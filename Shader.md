> ### 基础结构

```
/// 定义Shader的目录层次结构
Shader "DDYShader/DDYShaderProfile"
{
    /// 属性，定义渲染的输入数据
    Properties
    {
        // 属性名称("Inspector面板显示文本", 数据类型) = 默认值
        // 2D:2D纹理贴图，可为代表tint颜色的字符串，还可以空字符串或"white","black","gray","bump", 材质输入框，包含UV输入框
        _MainTexture("Sprite Texture", 2D) = "white" {}
        // Color:RGBA颜色
        _MainColor("Tint", Color) = (1,1,1,1)
        // int:整型
        _ShowShadow("Show Shadow", Int) = 1
        // Float:浮点型
        _RoundedRadius("Rounded Radius", Float) = 8
        // Range:范围浮点数
        _BackCut("Back cutoff", Range (0, 1)) = 0
        // 3D:3D贴图 
        _ModelTexture("Model Texture", 3D) = "black" {}
        // Cube:6面立方贴图
        _CubeTexture("Cube Texture", Cube) = "gray" {}
        // Vector:四维向量
        _Vector("Vectore", Vector) = (1, 2, 5, 6)
        // Rect:矩形贴图
        _Rect("Rect", Rect) = "bump" {}
        
        // [HideInInspector]在Inspector面板隐藏该属性
        [HideInInspector] _WorkflowMode("工作流模式", Float) = 1.0
        // [NoScaleOffset] 没有UV设置框
        [NoScaleOffset] _BackTexture("Back Texture", 2D) = "white" {}
        // [Normal] 该材质需要输入法线贴图
        [Normal] _Normal("Normal", 2D) = "white" {}
        // [HDR] 该材质需要高动态范围HDR纹理
        [HDR] _HDR ("HDR", 2D) = "white" {}
        // [Gamma] 在UI中将一个float/vector属性指定为sRGB值(像颜色一样)，可能需要根据使用的颜色空间进行转换
        [Gamma] _GammaVector("Vector", Vector) = (1, 1, 1, 1)
        // 指数滑杆
        [PowerSlider(3.0)] _Brightness("Brightness", Range (0.01, 1)) = 0.1
        
        // toggle勾选框
        // shader中没有bool类型，所以靠Float实现，但Float比较不可靠，可能发生 1.0!=1.00!=1/1的问题，最后使用相关函数辅助
        
    }

    SubShader
    {
        Tags
        {
            "Queue" = "Transparent"
            "IgnoreProjector" = "True"
            "RenderType" = "Transparent"
            "PreviewType" = "Plane"
            "CanUseSpriteAtlas" = "True"
        }

        Stencil
        {
            Ref[_Stencil]
            Comp[_StencilComp]
            Pass[_StencilOp]
            ReadMask[_StencilReadMask]
            WriteMask[_StencilWriteMask]
        }

        Cull Off
        Lighting Off
        ZWrite Off
        ZTest[unity_GUIZTestMode]
        Blend SrcAlpha OneMinusSrcAlpha
        ColorMask[_ColorMask]

        Pass
        {
            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag

            #include "UnityCG.cginc"
            #include "UnityUI.cginc"

            #pragma multi_compile __ UNITY_UI_ALPHACLIP

            struct appdata_t
            {
                float4 vertex : POSITION;
                float4 color : COLOR;
                float2 texcoord : TEXCOORD0;
            };

            struct v2f
            {
                float4 vertex : SV_POSITION;
                fixed4 color : COLOR;
                half2 texcoord : TEXCOORD0;
                float4 worldPosition : TEXCOORD1;
            };

            fixed4 _Color;
            fixed4 _TextureSampleAdd;
            float4 _ClipRect;
            float _RoundedRadius;
            float4 _MainTex_TexelSize;
            float _Width;
            float _Height;
            int _LeftTop;
            int _RightTop;
            int _LeftBottom;
            int _RightBottom;

            v2f vert(appdata_t IN)
            {
                v2f OUT;
                OUT.worldPosition = IN.vertex;
                OUT.vertex = UnityObjectToClipPos(OUT.worldPosition);

                OUT.texcoord = IN.texcoord;

                #ifdef UNITY_HALF_TEXEL_OFFSET
					OUT.vertex.xy += (_ScreenParams.zw - 1.0) * float2(-1 , 1);
                #endif

                OUT.color = IN.color * _Color;
                return OUT;
            }

            sampler2D _MainTex;

            fixed4 frag(v2f IN) : SV_Target
            {
                half4 color = (tex2D(_MainTex, IN.texcoord) + _TextureSampleAdd) * IN.color;

                color.a *= UnityGet2DClipping(IN.worldPosition.xy, _ClipRect);

                #ifdef UNITY_UI_ALPHACLIP
					clip(color.a - 0.001);
                #endif

                // 切圆角+抗锯齿
                float width = max(0.001, _Width);
                float height = max(0.001, _Height);

                float x = IN.texcoord.x * width;
                float y = IN.texcoord.y * height;
                // 半径大于等于0且小于宽高小值的一半
                float r = min(min(width, height) / 2.0f, max(_RoundedRadius, 0));

                float antiAliasingVal = r;

                // 水平垂直坐标对比半径
                float xrVal = step(x, r);
                float yrVal = step(y, r);
                float yhrVal = step(y, height - r);
                float xwrVal = step(x, width - r);

                // 左下角距离参数
                float lb_dis = (x - r) * (x - r) + (y - r) * (y - r);

                // 左上角距离参数
                float lt_dis = (x - r) * (x - r) + (y - (height - r)) * (y - (height - r));

                // 右下角距离参数
                float rb_dis = (x - (width - r)) * (x - (width - r)) + (y - r) * (y - r);

                // 右上角距离参数
                float rt_dis = (x - (width - r)) * (x - (width - r)) + (y - (height - r)) * (y - (height - r));

                // 像素点所在位置距离
                float dis = _LeftBottom * xrVal * yrVal * lb_dis +
                    _LeftTop * xrVal * (1 - yhrVal) * lt_dis +
                    _RightBottom * (1 - xwrVal) * yrVal * rb_dis +
                    _RightTop * (1 - xwrVal) * (1 - yhrVal) * rt_dis;

                // 标准边缘的距离对比
                float low = step(dis, max(r * r - 3 * antiAliasingVal, 0));
                float nor = step(dis, max(r * r - 2 * antiAliasingVal, 0));
                float high = step(dis, max(r * r - antiAliasingVal, 0));
                float max = step(dis, r * r);

                // 赋值透明度渐变像素(圆角+抗锯齿)
                color.a = (nor * (1 - low) * 0.8 + (1 - nor) * high * 0.5 + (1 - high) * max * 0.2 + (1 - max) * 0 +
                    low) * color.a;

                return color;
            }
            ENDCG
        }
    }
}
```

shader中没有bool类型，相关功能靠Float实现，但是Float比较不可靠，可能出现 1.0!=1.00!=1/1的情况，所以最后使用相关函数实现

Toggle勾选框

```
Properties
{
    // 声明开关
    [Toggle] _Toggle("_Toggle", Float) = 1.0
}
fixed4 frag (v2f i) : SV_Target
{
	// 使用
	if (_Toggle) {
          
    } else {
          
    }
}
```

MaterialToggle开关勾选框

```
Properties 
{
   [MaterialToggle] _Toggle("Enable", Float) = 1
}
fixed4 frag (v2f i) : SV_Target
{
    // 作相应处理
    if(_Toggle==0){
       
    } else {
    
    }
}
```

MaterialToggle(_DefinedName) 宏开关勾选框
该宏可在pass任意位置使用，还可以通过C# material.setKey("key", value)赋值

```
Properties {
	// 显示名为"Enable"，类型为Float，shader中使用_TEX_ON
	[MaterialToggle(_TEX_ON)] _Toggle("Enable", Float) = 0
}

Pass {
   // 声明bool宏：_TEX_ON
   #pragma multi_compile _TEX_ON
	// 动态声明变量
	#if _TEX_ON
	sampler2D _MainTex;
   fixed4 _TargetColor;
	#endif
	
	fixed4 frag (v2f i) : SV_Target
	{
		fixed4 col = tex2D(_MainTex, i.uv);
		// 动态处理代码片段
	    #if _TEX_ON
	       col = fixed4(1,0,0,1);
	    #else
			col = fixed4(0,1,0,1);
		#endif
	};
```

枚举属性会在Inspector面板生成下拉框，值为浮点型

使用内置枚举

```
Properties
{
	// 声明Unity内置的枚举类
	[Enum(UnityEngine.Rendering.BlendMode)] _SrcBlend ("Src Blend Mode", Float) = 1
	[Enum(UnityEngine.Rendering.BlendMode)] _DstBlend ("Dst Blend Mode", Float) = 1
	[Enum(UnityEngine.Rendering.CullMode)] _Cull ("Cull Mode", Float) = 1
	[Enum(UnityEngine.Rendering.CompareFunction)] _ZTest ("ZTest", Float) = 0
}
SubShader
{
	// 使用枚举
	Tags { "Queue"="Transparent" "RenderType"="Transparent" }
	Blend [_SrcBlend] [_DstBlend]
	Cull [_Cull]
	ZTest [_ZTest]
	ZWrite [_ZWrite]
}
```

使用自定义枚举

```
Properties
{
	// 自定义枚举的名称数值对,最多支持7对
	[Enum(Off, 0, On, 1)] _Cull("ZWrite", Float) = 0
}
SubShader
{
	Cull [_Cull]
}	
```

使用关键字枚举

```
Properties
{
		[KeywordEnum(NONE, A, B)] _Opt ("Opt mode", Float) = 0
}
SubShader
{
	// 声明枚举关键字
	// 关键词格式为：name_枚举名称，必须大写。最多支持9个
	#pragma multi_compile _Opt_NONE _Opt_A _Opt_B
	
	fixed4 frag (v2f i) : SV_Target
	{
		fixed4 secCol = tex2D(_SecondTex, i.uv.zw);
		// 使用关键字枚举
		#if _Opt_NONE 
			col += secCol;
		#elif _Opt_A 
			col *= secCol;
		#endif
		return col;
	}
}	
```

使用cs脚本扩展shader面板ui

首先建立shader gui的edtor脚本

```
using System;
using UnityEngine;
using UnityEditor;

public class MyShaderGUI : ShaderGUI
{
    public override void OnGUI(MaterialEditor materialEditor, MaterialProperty[] properties)
    {
        // 渲染shader自带的ui
        base.OnGUI(materialEditor, properties);
		// 当前关联的材质
        Material targetMat = materialEditor.target as Material;
		// 定义勾选框变量
        bool CS_BOOL = Array.IndexOf(targetMat.shaderKeywords, "CS_BOOL") != -1;
		// 发生改变后
        EditorGUI.BeginChangeCheck();
        CS_BOOL = EditorGUILayout.Toggle("CS_BOOL", CS_BOOL);

        if (EditorGUI.EndChangeCheck())
        {
            //控制shader中的变量
            if (CS_BOOL)
                targetMat.EnableKeyword("CS_BOOL");
            else
                targetMat.DisableKeyword("CS_BOOL");
        }
    }
}
```

然后在shader中引用edtor脚本和shader变量

```
Shader "Custom/MyShaderGUI"
{
    Properties
    {
        _MainTex("Texture", 2D) = "white" {}
    }
    SubShader
    {
        Tags{ "RenderType" = "Opaque" }
        LOD 200
        CGPROGRAM
        #pragma surface surf Lambert addshadow
        // 创建被cs脚本引用的宏
        #pragma shader_feature CS_BOOL
        sampler2D _MainTex;
        struct Input
        {
            float2 uv_MainTex;
        };
        void surf(Input IN, inout SurfaceOutput o)
        {
            half4 c = tex2D(_MainTex, IN.uv_MainTex);
            o.Albedo = c.rgb;
            o.Alpha = c.a;
			// 使用 脚本宏控制bool变量
            #if CS_BOOL
            o.Albedo.a = 0;
            #endif
        }
        ENDCG
    }
    // 关联gui cs脚本
    CustomEditor "MyShaderGUI"
}
```

* [shader入门数学基础矩阵篇](https://zhuanlan.zhihu.com/p/60415361)
* [unity3d Shader:Ugui Text 穿透模型与背景显示](https://blog.csdn.net/luoyikun/article/details/79925576)
* [逐顶点漫反射](https://blog.csdn.net/ABigDeal/article/details/81334801)
* [逐像素漫反射](https://blog.csdn.net/ABigDeal/article/details/81450747)
* [逐顶点高光反射](https://blog.csdn.net/ABigDeal/article/details/81504881)
* [逐像素高光反射](https://blog.csdn.net/ABigDeal/article/details/81537114)
* [单张纹理](https://blog.csdn.net/ABigDeal/article/details/81662899)
* [凹凸映射](https://blog.csdn.net/ABigDeal/article/details/82256881)
* [渐变纹理](https://blog.csdn.net/ABigDeal/article/details/82256949)
* [遮罩纹理](https://blog.csdn.net/ABigDeal/article/details/82256962)
* [透明度测试](https://blog.csdn.net/ABigDeal/article/details/82256992)
* [透明度混合](https://blog.csdn.net/ABigDeal/article/details/82257018)
* [UnityShader笔记](https://blog.csdn.net/u013477973/category_7719654.html)
* [shader教程 GitHub](https://github.com/JiepengTan/FishManShaderTutorial)
* [关于炫酷的Unity3D Shader | About Cool Unity3D Shaders](https://github.com/QianMo/Awesome-Unity-Shader)
* [ShaderStudy github](https://github.com/lovepurple/ShaderStudy)
* [unity 切圆角矩形 --shader编程](https://blog.csdn.net/fengya1/article/details/50771629)
* [Unity裁剪四方形Image为圆形](https://www.cnblogs.com/cocotang/p/7308411.html)
* [圆角Shader 支持单个圆角](https://github.com/MingUnity/MGFramework/blob/4e0bf95961115e74a8dbf5a8a32761b5c6ba9ca8/MGFrameworkProject/Assets/MGFramework/Shaders/UIRoundRect.shader)