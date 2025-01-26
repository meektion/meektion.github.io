---
title: seesion登录校验
date: 2025-01-26
categories: 
    - Java
cover: https://gcore.jsdelivr.net/gh/WQhuanm/Img_repo_1@main/img/202412222015910.png
---

### 1. 合法用户登录时，会将其用户信息存到session中 
~~~java
session.setAttribute("user",user);
~~~
### 2. 当处理需要权限的请求时，拦截器会查看session是否存在用户信息，有则放行

```java
public class LoginInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
       //1.获取session中的用户
        HttpSession session = request.getSession();
        Object user = session.getAttribute("user");
        //3.判断用户是否存在
        if(user == null){
              //4.不存在，拦截，返回401状态码
              response.setStatus(401);
              return false;
        }
        //5.存在，保存用户信息到Threadlocal,以便后面从线程提取用户信息
        UserHolder.saveUser((User)user);
        return true;
    }
}

//附UserHolder类
public class UserHolder {
    private static final ThreadLocal<UserDTO> tl = new ThreadLocal<>();
    public static void saveUser(UserDTO user){
        tl.set(user);
    }
    public static UserDTO getUser(){
        return tl.get();
    }
    public static void removeUser(){
        tl.remove();
    }
}
```

