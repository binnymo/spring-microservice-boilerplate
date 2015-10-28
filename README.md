# spring-rest-oauth2-sample

[中文版](README_zh.md)

## Index

- [Build and Run](#build)
- [Usage](#usage)
  - [Use the seed](#seed)
  - [Get **access_token**](#access_token)
  - [Get new **access_token** with **refresh_token**](#refresh_token)
  - [Access to welcome resource](#welcome)
  - [Access to user resource](#user)
    - [1. Create User](#create)
    - [2. Show all users](#all)
    - [3. Show users in page](#page)
    - [4. Show user by user's ID](#show_by_id)
    - [5. Show user by user's usr](#show_by_usr)
    - [6. Update user by user's ID](#update)
    - [7. Delete user by user's ID](#delete)
  - [Other resources](#other)
- [License](#license)
- [Version History](#version)

This is REST service sample be supported by:

- [Spring Boot](http://projects.spring.io/spring-boot/)
- [Spring OAuth 2](http://projects.spring.io/spring-security-oauth/)
- [Spring Security](http://projects.spring.io/spring-security/)
- [Spring Data JPA](http://projects.spring.io/spring-data-jpa/)

## <span id="build">Build and Run</span>

```
$ cd <spring-rest-oauth2-sample root path>
$ ./gradlew clean build bootRun
```

## <span id="usage">Usage</span>

### <span id="seed">Use the [Seed](src/test/java/com/saintdan/framework/repo/Seed.java)</span>

### <span id="access_token">Get access_token</span>

Take your token from `oauth/token` in terminal, if you use ssl remember add `-k`:

```
$ curl -X POST -vu ios_app:123456 http://localhost:8080/oauth/token -H "Accept: application/json" -d "password=admin&username=admin&grant_type=password&scope=read&client_secret=123456&client_id=ios_app"
```

or [Advanced REST Client](https://github.com/jarrodek/advanced-rest-client) in your Chrome with:

```
url: http://localhost:8080/oauth/token
POST
headers: Authorization: Basic <Encrypt client_id:client_secret by HTTP Basic>
playload: password=admin&username=admin&grant_type=password&scope=read
```

### <span id="refresh_token">Get new **access_token** with **refresh_token**</span>

```
curl -X POST -vu ios_app:123456 http://localhost:8080/oauth/token -H "Accept: application/json" -d "grant_type=refresh_token&refresh_token=<refresh_token_returned>&client_secret=123456&client_id=ios_app"
```

or use Advanced REST Client:

```
url: http://localhost:8080/oauth/token
POST
headers: Authorization: Basic <Encrypt client_id:client_secret by HTTP Basic>
playload: grant_type=refresh_token&refresh_token=<refresh_token_returned>
```

### <span id="welcome">Access to welcome resource</span>

Use the **access_token** returned to make the authorized request to the protected endpoint:

```
$ curl http://localhost:8080/welcome -H "Authorization: Bearer <access_token_returned>"
```
If the request is successful, you will see the following JSON response:

```
{
	"id":2,
	"content":"Hello, admin!"
}
```

or use Advanced REST Client:

```
url: http://localhost:8080/welcome
GET
headers: Authorization: bearer <access_token_returned>
```

### <span id="user">Access to user resource</span>

#### <span id="create">1. Create new user</span>

```
curl -X POST "http://localhost:8080/resources/users/sign=QWtrbUdQJTJGcmpQcWRGcHpPWWVRUk1BWXNLJTJGWFN5b2Z3RFNOUVNuQ1drNiUyQjQ5dHB0amZzaGpIVER0WWhxOEU2bUpmZkVBbHk1NW9uVDdNSEFjOGpsalhSeVduY3puMzVXQkMwUE1UZ1BYZXdORG9LY0xMV2FKTzZXJTJGY0NhbTh6THolMkYzQ3I2JTJGQiUyQllTS0ZOQ1NVMCUyQndLN3JBM1JtTEVuRWNrTyUyQjBreGNtZVNEUFk5bVFhRUNJNWd2eG5wWUVoUE02WDVMV01Kak55VkdsTjQ5ZUJHUm42a3dYbHQzbWxzTjBwT2ozQWdkTHlHVkZhdTJMaWdjRHljNGQzaFpnMmtvbiUyQkhZTDRMN2VuZE91VWFRaXFIOU1iaWtwdUxjOFhNbHhYOFVUUW1CSHVzTno2SkZzciUyQlFtRkRyT1hUdlN3d2dsSFl3Tlk1Qk55SGRlZlNGMWh0OVJhZyUzRCUzRA==" -H "Authorization: bearer <access_token_returned>" -d "usr=tom&name=tom&pwd=tom"
```

If the request is successful, you will see the following JSON response:

```
{
    "code": "200", 
    "operationStatus": "SUCCESS", 
    "message": "Create user successfully.", 
    "id": 4, 
    "name": "tom", 
    "usr": "tom", 
    "description": null
}
```

or use Advanced REST Client:

```
url: http://localhost:8080/resources/users/sign=QWtrbUdQJTJGcmpQcWRGcHpPWWVRUk1BWXNLJTJGWFN5b2Z3RFNOUVNuQ1drNiUyQjQ5dHB0amZzaGpIVER0WWhxOEU2bUpmZkVBbHk1NW9uVDdNSEFjOGpsalhSeVduY3puMzVXQkMwUE1UZ1BYZXdORG9LY0xMV2FKTzZXJTJGY0NhbTh6THolMkYzQ3I2JTJGQiUyQllTS0ZOQ1NVMCUyQndLN3JBM1JtTEVuRWNrTyUyQjBreGNtZVNEUFk5bVFhRUNJNWd2eG5wWUVoUE02WDVMV01Kak55VkdsTjQ5ZUJHUm42a3dYbHQzbWxzTjBwT2ozQWdkTHlHVkZhdTJMaWdjRHljNGQzaFpnMmtvbiUyQkhZTDRMN2VuZE91VWFRaXFIOU1iaWtwdUxjOFhNbHhYOFVUUW1CSHVzTno2SkZzciUyQlFtRkRyT1hUdlN3d2dsSFl3Tlk1Qk55SGRlZlNGMWh0OVJhZyUzRCUzRA==
POST
headers: Authorization: bearer <access_token_returned>
payload: usr=tom&name=tom&pwd=tom
```

#### 2. <span id="all">Show all users</span>

```
$ curl "http://localhost:8080/resources/users/sign=dWFOOUVDdXFnYmMyUG55TXJ0aVlNbHpEOEdzQ2VpSHFRNVloRlE3ajBlMGUxcHRrYXB2T2RqSjJWdjczQ0tJaFVrJTJGckhwdXEzT0lRdDJadzF3ZnVONzhoV3JBMkNZTjBSV24lMkJtSW95MjJlRDYxOCUyRkpjNWpTWjRGQXBuOGw4Zzl6VExKN3dHVEU0T1VGeU5vS1ZDcldJSmFkT3NPSk0wNFFjWXMyNTd3ZmtHSCUyQnRCTHhXVVJEbjBZUEFwRjd1amtKZ3FUUW83d3o4VmlWdVY1NThnM3BKZ2QlMkIlMkZNWUV2MEpzTmMlMkZKNkRHaGhROWR5Z2VFRExJUHdzZjZ0dkZqVFlvRVFrY1B2WmhQV0p1R0VEMjZSZHRnVFdWTlNObyUyQjVmZU9MSHBENW9TSHVyVXdrM2FtMFlqTUQyRFoyRkdmek91WkI1NzhrWTZIc1VUUGhvU0diajBzdyUzRCUzRA==" -H "Authorization: Bearer <access_token_returned>"
```

If the request is successful, you will see the following JSON response:

```
{
    "code": "200", 
    "operationStatus": "SUCCESS", 
    "message": "Get all users data successfully.", 
    "userVOList": [
        {
            "code": null, 
            "operationStatus": null, 
            "message": null, 
            "id": 1, 
            "name": "root", 
            "usr": "root", 
            "description": "root account"
        }, 
        {
            "code": null, 
            "operationStatus": null, 
            "message": null, 
            "id": 2, 
            "name": "admin", 
            "usr": "admin", 
            "description": "admin account"
        }, 
        {
            "code": null, 
            "operationStatus": null, 
            "message": null, 
            "id": 3, 
            "name": "guest", 
            "usr": "guest", 
            "description": "guest account"
        }, 
        {
            "code": null, 
            "operationStatus": null, 
            "message": null, 
            "id": 4, 
            "name": "tom", 
            "usr": "tom", 
            "description": null
        }
    ]
}
```

or use Advanced REST Client:

```
url: http://localhost:8080/resources/users/sign=dWFOOUVDdXFnYmMyUG55TXJ0aVlNbHpEOEdzQ2VpSHFRNVloRlE3ajBlMGUxcHRrYXB2T2RqSjJWdjczQ0tJaFVrJTJGckhwdXEzT0lRdDJadzF3ZnVONzhoV3JBMkNZTjBSV24lMkJtSW95MjJlRDYxOCUyRkpjNWpTWjRGQXBuOGw4Zzl6VExKN3dHVEU0T1VGeU5vS1ZDcldJSmFkT3NPSk0wNFFjWXMyNTd3ZmtHSCUyQnRCTHhXVVJEbjBZUEFwRjd1amtKZ3FUUW83d3o4VmlWdVY1NThnM3BKZ2QlMkIlMkZNWUV2MEpzTmMlMkZKNkRHaGhROWR5Z2VFRExJUHdzZjZ0dkZqVFlvRVFrY1B2WmhQV0p1R0VEMjZSZHRnVFdWTlNObyUyQjVmZU9MSHBENW9TSHVyVXdrM2FtMFlqTUQyRFoyRkdmek91WkI1NzhrWTZIc1VUUGhvU0diajBzdyUzRCUzRA==
GET
headers: Authorization: bearer <access_token_returned>
```

#### <span id="page">3. Show users in page</span>

```
$ curl "http://localhost:8080/resources/users/pageNo=0/sign=dWFOOUVDdXFnYmMyUG55TXJ0aVlNbHpEOEdzQ2VpSHFRNVloRlE3ajBlMGUxcHRrYXB2T2RqSjJWdjczQ0tJaFVrJTJGckhwdXEzT0lRdDJadzF3ZnVONzhoV3JBMkNZTjBSV24lMkJtSW95MjJlRDYxOCUyRkpjNWpTWjRGQXBuOGw4Zzl6VExKN3dHVEU0T1VGeU5vS1ZDcldJSmFkT3NPSk0wNFFjWXMyNTd3ZmtHSCUyQnRCTHhXVVJEbjBZUEFwRjd1amtKZ3FUUW83d3o4VmlWdVY1NThnM3BKZ2QlMkIlMkZNWUV2MEpzTmMlMkZKNkRHaGhROWR5Z2VFRExJUHdzZjZ0dkZqVFlvRVFrY1B2WmhQV0p1R0VEMjZSZHRnVFdWTlNObyUyQjVmZU9MSHBENW9TSHVyVXdrM2FtMFlqTUQyRFoyRkdmek91WkI1NzhrWTZIc1VUUGhvU0diajBzdyUzRCUzRA==" -H "Authorization: Bearer <access_token_returned>"
```

If the request is successful, you will see the following JSON response:

```
{
    "code": "200", 
    "operationStatus": "SUCCESS", 
    "message": "Get all users data successfully.", 
    "page": {
        "content": [
            {
                "code": null, 
                "operationStatus": null, 
                "message": null, 
                "id": 1, 
                "name": "root", 
                "usr": "root", 
                "description": "root account"
            }, 
            {
                "code": null, 
                "operationStatus": null, 
                "message": null, 
                "id": 2, 
                "name": "admin", 
                "usr": "admin", 
                "description": "admin account"
            }, 
            {
                "code": null, 
                "operationStatus": null, 
                "message": null, 
                "id": 3, 
                "name": "guest", 
                "usr": "guest", 
                "description": "guest account"
            },
            {
            "code": null, 
            "operationStatus": null, 
            "message": null, 
            "id": 4, 
            "name": "tom", 
            "usr": "tom", 
            "description": null
            }
        ], 
        "totalElements": 4, 
        "totalPages": 1, 
        "last": true, 
        "size": 20, 
        "number": 0, 
        "first": true, 
        "sort": null, 
        "numberOfElements": 4
    }
}
```

or use Advanced REST Client:

```
url: http://localhost:8080/resources/users/pageNo=0/sign=dWFOOUVDdXFnYmMyUG55TXJ0aVlNbHpEOEdzQ2VpSHFRNVloRlE3ajBlMGUxcHRrYXB2T2RqSjJWdjczQ0tJaFVrJTJGckhwdXEzT0lRdDJadzF3ZnVONzhoV3JBMkNZTjBSV24lMkJtSW95MjJlRDYxOCUyRkpjNWpTWjRGQXBuOGw4Zzl6VExKN3dHVEU0T1VGeU5vS1ZDcldJSmFkT3NPSk0wNFFjWXMyNTd3ZmtHSCUyQnRCTHhXVVJEbjBZUEFwRjd1amtKZ3FUUW83d3o4VmlWdVY1NThnM3BKZ2QlMkIlMkZNWUV2MEpzTmMlMkZKNkRHaGhROWR5Z2VFRExJUHdzZjZ0dkZqVFlvRVFrY1B2WmhQV0p1R0VEMjZSZHRnVFdWTlNObyUyQjVmZU9MSHBENW9TSHVyVXdrM2FtMFlqTUQyRFoyRkdmek91WkI1NzhrWTZIc1VUUGhvU0diajBzdyUzRCUzRA==
GET
headers: Authorization: bearer <access_token_returned>
```

#### <span id="show_by_id">4. Show user by user's ID</span>

```
$ curl "http://localhost:8080/resources/users/4/sign=dWFOOUVDdXFnYmMyUG55TXJ0aVlNbHpEOEdzQ2VpSHFRNVloRlE3ajBlMGUxcHRrYXB2T2RqSjJWdjczQ0tJaFVrJTJGckhwdXEzT0lRdDJadzF3ZnVONzhoV3JBMkNZTjBSV24lMkJtSW95MjJlRDYxOCUyRkpjNWpTWjRGQXBuOGw4Zzl6VExKN3dHVEU0T1VGeU5vS1ZDcldJSmFkT3NPSk0wNFFjWXMyNTd3ZmtHSCUyQnRCTHhXVVJEbjBZUEFwRjd1amtKZ3FUUW83d3o4VmlWdVY1NThnM3BKZ2QlMkIlMkZNWUV2MEpzTmMlMkZKNkRHaGhROWR5Z2VFRExJUHdzZjZ0dkZqVFlvRVFrY1B2WmhQV0p1R0VEMjZSZHRnVFdWTlNObyUyQjVmZU9MSHBENW9TSHVyVXdrM2FtMFlqTUQyRFoyRkdmek91WkI1NzhrWTZIc1VUUGhvU0diajBzdyUzRCUzRA==" -H "Authorization: Bearer <access_token_returned>"
```

If the request is successful, you will see the following JSON response:

```
{
    "code": "200", 
    "operationStatus": "SUCCESS", 
    "message": "Get user data successfully.", 
    "id": 4, 
    "name": "tom", 
    "usr": "tom", 
    "description": null
}
```

or use Advanced REST Client:

```
url: http://localhost:8080/resources/users/4/sign=dWFOOUVDdXFnYmMyUG55TXJ0aVlNbHpEOEdzQ2VpSHFRNVloRlE3ajBlMGUxcHRrYXB2T2RqSjJWdjczQ0tJaFVrJTJGckhwdXEzT0lRdDJadzF3ZnVONzhoV3JBMkNZTjBSV24lMkJtSW95MjJlRDYxOCUyRkpjNWpTWjRGQXBuOGw4Zzl6VExKN3dHVEU0T1VGeU5vS1ZDcldJSmFkT3NPSk0wNFFjWXMyNTd3ZmtHSCUyQnRCTHhXVVJEbjBZUEFwRjd1amtKZ3FUUW83d3o4VmlWdVY1NThnM3BKZ2QlMkIlMkZNWUV2MEpzTmMlMkZKNkRHaGhROWR5Z2VFRExJUHdzZjZ0dkZqVFlvRVFrY1B2WmhQV0p1R0VEMjZSZHRnVFdWTlNObyUyQjVmZU9MSHBENW9TSHVyVXdrM2FtMFlqTUQyRFoyRkdmek91WkI1NzhrWTZIc1VUUGhvU0diajBzdyUzRCUzRA==
GET
headers: Authorization: bearer <access_token_returned>
```

#### <span id="show_by_usr">5. Show user by user's usr</span>

```
$ curl "http://localhost:8080/resources/users/usr=admin/sign=TEtjbkozTWVXNUxxOUJTYmxubUNJQkhqN0dPeE1RUzdqM0tURThsVXlJd29sQXMlMkZnTU1WejVrTklpTDA2ZVBMdExJJTJGZThLWUp0aiUyRlJDN3JockhkYm9GaHVFeUZZcHB2MEhwVTJ2OEoxYVoyYXJHZm1jWiUyQlBRJTJCdEFVQ016d2ZvSVhFV25mMG1zelJxMXNQMm43MVRrWnh1MiUyQjdrb1BQamNlJTJGTmw2RXZSdWpmb3Y1Ynh0JTJCZ2RtTHNGUllESFVZQU04NHBOdURoNmlvYWMyblFPdXFGeHhSeXNITXJkYklLQnhpYXFkcVVJY3NVQ1JvMDhJTVptaXFIVmNvJTJGWXNTRnRRMU4weFJvNjRaS2JxJTJCb3dZRkdvT1cxRDl4T0J3MzdWMUYxelNlRm5KZExONjBQNWwwSlg2VGtLeEw3M0JqSnRWcDZvaU1VZEJhdDgySDFFY3N6R0ElM0QlM0Q=" -H "Authorization: Bearer <access_token_returned>"
```

If the request is successful, you will see the following JSON response:

```
{
	code: "200"
	operationStatus: "SUCCESS"
	message: "Get user data successfully."
	name: "admin"
	username: "admin"
}
```

or use Advanced REST Client:

```
url: http://localhost:8080/resources/users/usr=admin/sign=TEtjbkozTWVXNUxxOUJTYmxubUNJQkhqN0dPeE1RUzdqM0tURThsVXlJd29sQXMlMkZnTU1WejVrTklpTDA2ZVBMdExJJTJGZThLWUp0aiUyRlJDN3JockhkYm9GaHVFeUZZcHB2MEhwVTJ2OEoxYVoyYXJHZm1jWiUyQlBRJTJCdEFVQ016d2ZvSVhFV25mMG1zelJxMXNQMm43MVRrWnh1MiUyQjdrb1BQamNlJTJGTmw2RXZSdWpmb3Y1Ynh0JTJCZ2RtTHNGUllESFVZQU04NHBOdURoNmlvYWMyblFPdXFGeHhSeXNITXJkYklLQnhpYXFkcVVJY3NVQ1JvMDhJTVptaXFIVmNvJTJGWXNTRnRRMU4weFJvNjRaS2JxJTJCb3dZRkdvT1cxRDl4T0J3MzdWMUYxelNlRm5KZExONjBQNWwwSlg2VGtLeEw3M0JqSnRWcDZvaU1VZEJhdDgySDFFY3N6R0ElM0QlM0Q=
GET
headers: Authorization: bearer <access_token_returned>
```

#### <span id="update">6. Update user by user's ID</span>

```
curl -X POST "http://localhost:8080/resources/users/4/sign=WTdORDVmcGxZcjh6cTVGU3Z0bkIlMkZYTSUyQjNkS0E0N3FHeWxsV0VyaFZ1UHNUUHg4Y2klMkZ0YnRhdVFUcUd2VmJYMXVpVXBBZU45VmJKSG03VVhIM3h2YVUwQ3pjJTJGZyUyQlhDMllrR25ybGVEQVlDbTFySWdWTG9Ca0x0Z2x3WTBRY3BHSjdudVdKSDQ2Y214dG8lMkI3VkNVbHljSTlwd3FkVGx4dk1rd2NtTEFFQnplSDg0RmpsUzBhVTg5JTJCa3AxbGRXNURYeFAyNVpNUnp5RU1xTnl3OWJnQlZTWXFnT3NVMUtPUTJVdmg5WEFmd2FyeWk2elRkeSUyRmpkOHRpRWRHaEUwcHl6Z2pwZzNuWnI2MWcwZW5ndVBHJTJGWXdDc25TdUc3STJBN29ZU1hPYTI4aG9WZWVLQ1F1NzZOT0xUMVk3YU1oUVZvJTJCSWdTVU1kaVdsaGZQaE9kaXB6UnclM0QlM0Q=" -H "Authorization: bearer <access_token_returned>" -d "usr=tom&name=jerry&pwd=tom"
```

If the request is successful, you will see the following JSON response:

```
{
    "code": "200", 
    "operationStatus": "SUCCESS", 
    "message": "Create user successfully.", 
    "id": 4, 
    "name": "tom", 
    "usr": "tom", 
    "description": null
}
```

or use Advanced REST Client:

```
url: http://localhost:8080/resources/users/4/sign=WTdORDVmcGxZcjh6cTVGU3Z0bkIlMkZYTSUyQjNkS0E0N3FHeWxsV0VyaFZ1UHNUUHg4Y2klMkZ0YnRhdVFUcUd2VmJYMXVpVXBBZU45VmJKSG03VVhIM3h2YVUwQ3pjJTJGZyUyQlhDMllrR25ybGVEQVlDbTFySWdWTG9Ca0x0Z2x3WTBRY3BHSjdudVdKSDQ2Y214dG8lMkI3VkNVbHljSTlwd3FkVGx4dk1rd2NtTEFFQnplSDg0RmpsUzBhVTg5JTJCa3AxbGRXNURYeFAyNVpNUnp5RU1xTnl3OWJnQlZTWXFnT3NVMUtPUTJVdmg5WEFmd2FyeWk2elRkeSUyRmpkOHRpRWRHaEUwcHl6Z2pwZzNuWnI2MWcwZW5ndVBHJTJGWXdDc25TdUc3STJBN29ZU1hPYTI4aG9WZWVLQ1F1NzZOT0xUMVk3YU1oUVZvJTJCSWdTVU1kaVdsaGZQaE9kaXB6UnclM0QlM0Q=
POST
headers: Authorization: bearer <access_token_returned>
payload: usr=tom&name=jerry&pwd=tom
```

#### <span id="delete">7. Delete user by user's ID</span>

```
curl -X DELETE "http://localhost:8080/resources/users/4/sign=dWFOOUVDdXFnYmMyUG55TXJ0aVlNbHpEOEdzQ2VpSHFRNVloRlE3ajBlMGUxcHRrYXB2T2RqSjJWdjczQ0tJaFVrJTJGckhwdXEzT0lRdDJadzF3ZnVONzhoV3JBMkNZTjBSV24lMkJtSW95MjJlRDYxOCUyRkpjNWpTWjRGQXBuOGw4Zzl6VExKN3dHVEU0T1VGeU5vS1ZDcldJSmFkT3NPSk0wNFFjWXMyNTd3ZmtHSCUyQnRCTHhXVVJEbjBZUEFwRjd1amtKZ3FUUW83d3o4VmlWdVY1NThnM3BKZ2QlMkIlMkZNWUV2MEpzTmMlMkZKNkRHaGhROWR5Z2VFRExJUHdzZjZ0dkZqVFlvRVFrY1B2WmhQV0p1R0VEMjZSZHRnVFdWTlNObyUyQjVmZU9MSHBENW9TSHVyVXdrM2FtMFlqTUQyRFoyRkdmek91WkI1NzhrWTZIc1VUUGhvU0diajBzdyUzRCUzRA==" -H "Authorization: bearer <access_token_returned>"
```

If the request is successful, you will see the following JSON response:

```
{
    "code": "200", 
    "operationStatus": "SUCCESS", 
    "message": "Delete user data successfully."
}
```

or use Advanced REST Client:

```
url: http://localhost:8080/resources/users/4/sign=dWFOOUVDdXFnYmMyUG55TXJ0aVlNbHpEOEdzQ2VpSHFRNVloRlE3ajBlMGUxcHRrYXB2T2RqSjJWdjczQ0tJaFVrJTJGckhwdXEzT0lRdDJadzF3ZnVONzhoV3JBMkNZTjBSV24lMkJtSW95MjJlRDYxOCUyRkpjNWpTWjRGQXBuOGw4Zzl6VExKN3dHVEU0T1VGeU5vS1ZDcldJSmFkT3NPSk0wNFFjWXMyNTd3ZmtHSCUyQnRCTHhXVVJEbjBZUEFwRjd1amtKZ3FUUW83d3o4VmlWdVY1NThnM3BKZ2QlMkIlMkZNWUV2MEpzTmMlMkZKNkRHaGhROWR5Z2VFRExJUHdzZjZ0dkZqVFlvRVFrY1B2WmhQV0p1R0VEMjZSZHRnVFdWTlNObyUyQjVmZU9MSHBENW9TSHVyVXdrM2FtMFlqTUQyRFoyRkdmek91WkI1NzhrWTZIc1VUUGhvU0diajBzdyUzRCUzRA==
DELETE
headers: Authorization: bearer <access_token_returned>
```

### <span id="other">Other resources</span>

Refer to previous user resource.
And you can generate the sign with [SignTest](src/test/java/com/saintdan/framework/tool/SignTest.java)

## <span id="license">License</span>

[MIT](http://opensource.org/licenses/MIT)

Copyright (c) 2015 saintdan

## <span id="version">Version History</span>

- 0.0.1.SNAPSHOT
  - Initial version.
  
- 0.1.0.RELEASE
  - Release version.
  
- 0.2.0.RELEASE
  - Change authorities to resources.
  - Add https.
  - Fix some bugs.
  
- 0.2.1.RELEASE
  - Add name to Resource PO.
  - Change getAuthorities().
  - Change import.sql.
  
- 0.3.0.RELEASE
  - Add /bo, /enums, /exception.
  - Add UserService and its implement.
  
- 0.3.1.RELEASE
  - Modify build.gradle.
  - Add refresh token usage.
  
- 0.3.2.RELEASE
  - Fix some hidden bugs.
  - Add SystemRuntimeException and UnknownException.
  
- 0.4.0.RELEASE
  - Add RSA signature.(You can generate your own RSA key pair with ssh-keygen, or get it in [GenerateRSAKeyPair](/src/test/java/com/saintdan/framework/GenerateRSAKeyPair.java).
  
- 0.4.1.RELEASE
  - Add [LogUtils](/src/main/java/com/saintdan/framework/tools/LogUtils.java) to trace info, warn, debug, error.
  
- 0.4.2.RELEASE
  - Optimize some codes.
  
- 0.4.3.RELEASE
  - Add [Base64ImageHelper](/src/main/java/com/saintdan/framework/tools/Base64ImageHelper.java).
  
- 0.4.4.RELEASE
  - Fix the error: "Could not find or load main class org.gradle.wrapper.GradleWrapperMain."  
    Add gradle-wrapper.jar
    Thanks [cbweixin](https://github.com/cbweixin) for reminding me.
    
- 0.5.0.RELEASE
  - Extract the elements with similar return results and integrate them into one --> [ResultHelper](/src/main/java/saintdan/framework/component/ResultHelper.java);
  - Extract the elements with similar signature and integrate them into one --> [SignHelper](/src/main/java/saintdan/framework/component/SignHelper.java);
  - Add package of RESTFul parameters.
  - Optimize code of services, implements, controllers.
  
- 0.5.1.RELEASE
  - Add success result response to [ResultHelper](/src/main/java/saintdan/framework/component/ResultHelper.java);
  - Optimize code of user service and controller.
  - Update Spring Boot to 2.0.7.RELEASE
  - Fix the signature bugs, and changes the test sign.
  
- 0.6.0.RELEASE
  - Add Maven support.
  
- 0.7.0.RELEASE
  - Add MySql support.
  
- 0.8.0.RELEASE
  - Add CRUD of user, role, group, resource.
  - Add some components, constants, exceptions.
  - Add [Seed](src/test/java/com/saintdan/framework/repo/Seed.java).
  
- 0.8.1.RELEASE
  - Modify table's name.
  
- 0.8.2.RELEASE
  - Add [CustomClientDetailsService](src/main/java/com/saintdan/framework/config/custom/CustomClientDetailsService.java), add jdbc client choice.
  - Add createdBy, createdDate, lastModifiedBy, lastModifiedDate to POs.
  - Add [SpringSecurityUtils](src/main/java/com/saintdan/framework/tools/SpringSecurityUtils.java), trace user's ip address.
  - Modify [Seed](src/test/java/com/saintdan/framework/repo/Seed.java).
  
- 0.8.3.RELEASE
  - Extract the elements with similar xxxsVO into one -> [ObjectsVO](src/main/java/com/saintdan/framework/vo/ObjectsVO.java).
  - Add show all resources in page.
  - Add log resource, hide the update & delete interface.