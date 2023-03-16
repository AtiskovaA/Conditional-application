# Conditional-application

Использовали упрощённые варианты наших сервисов. 

Написано приложение на Spring Boot, в котором в зависимости от параметров вызываютсяя разные сервисы. 

Для работы подготовлено несколько классов.
Создан интерфейс, реализации которого мы вызываем в зависимости от параметров приложения:

```$java
public interface SystemProfile {
     String getProfile();
}
``` 

Также созданы две реализации.
Первая:

```$java
public class DevProfile implements SystemProfile {
     @Override
     public String getProfile() {
         return "Current profile is dev";
     }
}
``` 

И вторая:

```$java
public class ProductionProfile implements SystemProfile {
     @Override
     public String getProfile() {
         return "Current profile is production";
     }
}
``` 

Написан JavaConfig, в котором объявлены бины классов `DevProfile` и `ProductionProfile`:

```$java
    @Bean
    public SystemProfile devProfile() {
        return new DevProfile();
    }

    @Bean
    public SystemProfile prodProfile() {
        return new ProductionProfile();
    }
```
    
Создан application.properties в папке resources с  пользовательским параметром `netology.profile.dev = true`.

Использовали одну из аннотаций @Conditional и поместили её на бины в JavaConfig (@ConditionalOnProperty).

Для проверки работоспособности логики, создан контроллер, который возвращает значения из сервиса `SystemProfile`:

```$java
@RestController
@RequestMapping("/")
public class ProfileController {
    private SystemProfile profile;

    public ProfileController(SystemProfile profile) {
        this.profile = profile;
    }

    @GetMapping("profile")
    public String getProfile() {
        return profile.getProfile();
    }
}
```
