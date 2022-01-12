

> ### 禁用自动烘焙

```
using UnityEditor;
using UnityEditor.SceneManagement;
using UnityEngine.SceneManagement;
 
[InitializeOnLoad]
public static class EditorDisableAutoGenerateLighting
{
    static EditorDisableAutoGenerateLighting()
    {
        EditorSceneManager.sceneOpened += OnSceneOpend;
        EditorSceneManager.newSceneCreated += OnSceneNewCreate;
    }
 
    private static void DisableAutoGenerateLighting()
    {
        Lightmapping.giWorkflowMode = Lightmapping.GIWorkflowMode.OnDemand;
    }
 
    public static void OnSceneOpend(Scene scene, OpenSceneMode mode)
    {
        DisableAutoGenerateLighting();
    }
 
    public static void OnSceneNewCreate(Scene scene, NewSceneSetup setup, NewSceneMode mode)
    {
        DisableAutoGenerateLighting();
    }
}
```