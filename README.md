# Unity-Singleton
Unity Singleton Presets! Just inherit from your preferred class. ALL credit goes to Tarodev. Check out his Patreon: https://www.patreon.com/tarodev

## Credit
Tarodev **Video Reference**: https://youtu.be/tE1qH8OxO2Y?t=231  
Check out his **Patreon**: https://www.patreon.com/tarodev  
Check out his **YouTube**: https://youtube.com/@Tarodev

## Singleton Types

### Static Instance
A static instance is similar to a singleton, but instead of destroying any new instances, it overrides the current instance. This is handy for resetting the state and saves you doing it manually.

```csharp
public abstract class StaticInstance<T> : MonoBehaviour
    where T : MonoBehaviour
{
    public static T Instance { get; private set; }

    protected virtual void Awake() => Instance = this as T;

    protected virtual void OnApplicationQuit()
    {
        Instance = null;
        Destroy(gameObject);
    }
}
```

### Singleton
This transforms the static instance into a basic singleton. This will destroy any new versions created, leaving the original instance intact.

```csharp
public abstract class Singleton<T> : StaticInstance<T>
    where T : MonoBehaviour
{
    protected override void Awake()
    {
        if (Instance != null)
        {
            Destroy(gameObject);
            return;
        }

        base.Awake();
    }
}
```

### Persistent Singleton
Finally, we have the persistent version of the singleton. This will survive through scene loads. Perfect for system classes which require stateful, persistent data. Or audio sources where music plays through loading scenes, etc.

```csharp
public abstract class PersistentSingleton<T> : Singleton<T>
    where T : MonoBehaviour
{
    protected override void Awake()
    {
        base.Awake();
        DontDestroyOnLoad(gameObject);
    }
}
```