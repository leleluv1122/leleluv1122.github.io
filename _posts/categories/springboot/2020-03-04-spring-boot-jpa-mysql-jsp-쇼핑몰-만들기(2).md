---
title: "spring boot & jpa & mysql & jsp ���θ� �����(2)"
date: 2020-03-04 00:55:35 -0400
categories: springboot
---

spring boot ���θ������ !
sts3, mysql, jpa, jsp�� ����Ͽ����ϴ�.

## 1) Mysql User���̺� �����ϱ�
 workbench �� �ͼ� ������ schema�� ���� Tables�� �����ʸ��콺�� Ŭ�� �� create table !

 ![8](../../../assets/8.JPG)

 �̿� ���� �ۼ����ݴϴ� �׸��� apply > apply 

## 2) lombok ��ġ
 lombok�� getter, setter �޼ҵ带 �ڵ����� �������༭ �������� �ʾƵ� �ȴ�
 
 Ŭ������  `@Data` ������̼Ǹ� �ٿ��ָ� �ȴ�!

 ����, https://projectlombok.org/download.html �� ���� lombok.jar�� �ٿ�ε��Ѵ�

 ![9](../../../assets/9.JPG) 

 lombok.jar�� ���� ���� ����ó�� specify location... > sts��ġ�������� sts.exe�� Ŭ���ϰ� select�Ѵ�

 �����ð��� ��ġ�� �Ϸ�Ǿ��� ��ġ�� �Ǿ����� Ȯ���ϰ������

![10](../../../assets/10.JPG) 

sts������ ���� STS.ini / SpringToolSuite3.ini ������ ���뿡�� ������ģ �κ��� ������ ���� ������ �Է��Ѵ�


## 3) Java Ŭ���� ����
### 1. JPA Entity Ŭ���� ����
 db�� �����ߴ� user ���̺��� �����غ��Կ�

 ��ġ: src/main/java/net/lele/domain/User.java

 - User.java 
```java
package net.lele.domain;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

import lombok.Data;

@Data
@Entity
public class User {
	@Id //primary key����
	@GeneratedValue(strategy = GenerationType.IDENTITY) //Auto increment����
	int id;

	int enable;
	String userId;

	String password;
	String userType;
	String name;
	String email;
	String phone;
	String address;
	String address_detail;
	int postcode;
	String addrplus;
}
```  

### 2. Repository Ŭ���� ����

 ��ġ: src/main/java/net/lele/repository/UserRepository.java

 - UserRepository.java
```java
package net.lele.repository;

import org.springframework.data.jpa.repository.JpaRepository;

import net.lele.domain.User;

public interface UserRepository extends JpaRepository<User, Integer> {

	User findOneByUserId(String userId); 
	// User ���̺��� userId�ʵ�� ���ڵ带 ��ȸ�ϴ� �޼ҵ�
	// jpa query creation ��ɿ� ���� �� �޼ҵ尡 �ڵ����� �����ȴ�
	// Ȥ�� DB�� userId�� ��ȸ�� ���ڵ尡 2�� �̻��̸� �� �޼ҵ�� ������ �߻��Ѵ�	

	User findByUserId(String userId);
}
```

### 3. Utility Ŭ����
 - ��ƿ��Ƽ Ŭ������ ������!!
  ���� ������Ʈ���� �������� ���� �� �ִ� ���� ����� ������ Ŭ����

  ��й�ȣ ��ȣȭ�� ��ȣȭ/��ȣȭ ����� ��ƿ��Ƽ Ŭ������ ��Ƽ� ����

 ��ġ: src/main/java/net/lele/utils/EncryptionUtils.java

 - EncryptionUtils.java

```java
package net.lele.utils;

import java.security.MessageDigest;

public class EncryptionUtils {

	public static String encryptSHA256(String s) { // �̰� �� ������
		return encrypt(s, "SHA-256");
	}

	public static String encryptMD5(String s) {
		return encrypt(s, "MD5");
	}

	public static String encrypt(String s, String messageDigest) {
	// messageDigest �Ķ����
	// �� �Ķ���ʹ� ��ȣȭ �˰����� �����Ѵ�
		try {
			MessageDigest md = MessageDigest.getInstance(messageDigest);
			//SHA-256 or MD5 �˰��� ��������
			byte[] passBytes = s.getBytes();
			md.reset();
			byte[] digested = md.digest(passBytes);
			StringBuffer sb = new StringBuffer();
			for (int i = 0; i < digested.length; i++)
				sb.append(Integer.toHexString(0xff & digested[i]));
			return sb.toString();
		} catch (Exception e) {
			return s;
		}
	}
}
```
### 4. Service Ŭ����
 - ���� Ŭ������ �� ������!
  ��Ʈ�ѷ� Ŭ������ ��ü���� �۾������� �ƴ� ���� ������ �ش��ϴ� �ڵ常 ����־���Ѵ�.
��ü���� �۾� ������ ���� Ŭ������ �����Ǿ�� �Ѵ�.

 ��ġ: src/main/java/net/lele/service/UserService.java

 - UserService.java

```java
package net.lele.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.validation.BindingResult;

import net.lele.domain.User;
import net.lele.model.UserRegistrationModel;
import net.lele.repository.UserRepository;
import net.lele.utils.EncryptionUtils;

@Service
public class UserService {
	@Autowired
	UserRepository userRepository;
	
	public User login(String userId, String password) {
		User user = userRepository.findOneByUserId(userId);
		if (user == null)
			return null;
		String pw = EncryptionUtils.encryptMD5(password); 
		// �н����带 ��ȣȭ�ؼ� ��ȣȭ�Ǽ� ����� db�� ���Ѵ�

		if (user.getPassword().equals(pw) == false)
			return null;
		return user;
	}

	public boolean hasErrors(UserRegistrationModel userModel, BindingResult bindingResult) {
		if (bindingResult.hasErrors())
			return true;
		if (userModel.getPasswd1().equals(userModel.getPasswd2()) == false) {
			bindingResult.rejectValue("passwd2", null, "��й�ȣ�� ��ġ���� �ʽ��ϴ�.");
			return true;
		}

		User user = userRepository.findOneByUserId(userModel.getUserid());
		if (user != null) {
			bindingResult.rejectValue("userid", null, "����� ���̵� �ߺ��˴ϴ�.");
			return true;
		}
		return false;
	}

	public User createEntity(UserRegistrationModel userModel) {
		User user = new User();
		String pw = EncryptionUtils.encryptMD5(userModel.getPasswd1());
		user.setUserId(userModel.getUserid());
		user.setPassword(pw);
		user.setUserType("user");
		user.setEnable(1);
		user.setEmail(userModel.getEmail());
		user.setAddress(userModel.getAddress());
		user.setName(userModel.getName());
		user.setPhone(userModel.getPhone());
		user.setAddress_detail(userModel.getAddress_detail());
		user.setPostcode(userModel.getPostcode());
		user.setAddrplus(userModel.getAddrplus());
		return user;
	}

	public void save(UserRegistrationModel userModel) {
		User user = createEntity(userModel);
		userRepository.save(user);
	}
}
```
 ȸ�����԰� �α��� ����� ����ϱ� ���� �ڵ���̴�.


 ��ġ: src/main/java/net/lele/service/MyAuthenticationProvider.java

 - MyAuthenticationProvider.java
```java
package net.lele.service;

import java.util.ArrayList;
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.stereotype.Component;

import net.lele.domain.User;

// ����ڰ� �Է��� �α��� ���̵�� ��й�ȣ�� �˻��� �� ���Ǵ� Ŭ����
@Component
public class MyAuthenticationProvider implements AuthenticationProvider {

    @Autowired UserService userService;

    @Override
    public Authentication authenticate(Authentication authentication) throws AuthenticationException {
        String loginId = authentication.getName();
        String passwd = authentication.getCredentials().toString();
        return authenticate(loginId, passwd);
    }
    // ����ڰ� �Է��� �α��� ���̵�� ��й�ȣ�� �˻��ؾ� �� �� �ڵ����� ȣ��
    // ����ڰ� �Է��� �α��� ���̵�� ��й�ȣ�� �� �޼ҵ��� �Ķ���ͷ� ���޵�

    public Authentication authenticate(String loginId, String password) throws AuthenticationException {
        User user = userService.login(loginId, password);
        if (user == null) return null; //�˻簡 �����ϸ� null ����

        List<GrantedAuthority> grantedAuthorities = new ArrayList<GrantedAuthority>();
        String role = "";
        switch (user.getUserType()) {
        case "admin": role = "ROLE_ADMIN"; break;
        case "user": role = "ROLE_STUDENT"; break;
        }
        // User ���̺��� userType �ʵ��� ���� '������', 'user'
        // spring security ������ ROLE_~~ �� �����Ѵ�
        grantedAuthorities.add(new SimpleGrantedAuthority(role));
        return new MyAuthenticaion(loginId, password, grantedAuthorities, user);
    }

    @Override
    public boolean supports(Class<?> authentication) {
        return authentication.equals(UsernamePasswordAuthenticationToken.class);
    }

    public class MyAuthenticaion extends UsernamePasswordAuthenticationToken {
        private static final long serialVersionUID = 1L;
        User user;

        public MyAuthenticaion (String loginId, String passwd,
                                List<GrantedAuthority> grantedAuthorities, User user) {
            super(loginId, passwd, grantedAuthorities);
            this.user = user;
        }

        public User getUser() {
            return user;
        }

        public void setUser(User user) {
            this.user = user;
        }
    }
}
```

### 5. Java Config Ŭ����

��ġ: src/main/java/net/lele/config/SecurityConfig.java

- SecurityConfig.java

```java
package net.lele.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.builders.WebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.web.util.matcher.AntPathRequestMatcher;

import net.lele.service.MyAuthenticationProvider;

@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {
	@Autowired
	MyAuthenticationProvider myAuthenticationProvider;

	@Override
	public void configure(WebSecurity web) throws Exception {
		web.ignoring().antMatchers("/res/**");
	}
	// /res/** ������ URL�� ���Ȱ˻縦 ���� ���� �����϶�� ���� (���ҽ����ϵ�)

	@Override
    protected void configure(HttpSecurity http) throws Exception
    {
        http.authorizeRequests() //���� ���� ����    /   ��>>>>�� - �켱����
			.antMatchers("/admin/**").hasAuthority("ROLE_ADMIN")   /* .access("ROLE_ADMIN") */
            //admin/** ������ ROLE_ADMIN���Ѽ����ڸ� ��û���� �ƴϸ� �źδ���
            .antMatchers("/guest/**").permitAll()
            // �α������� ���� ����ڵ� ���
            .antMatchers("/").permitAll()
            // ��� ����� ������
            .antMatchers("/**").authenticated();
        // /** ������ �α��ε� ����ڿ��Ը� ���

        http.csrf().disable();
        //CSRF ���� �˻������ʰڴ� ~_~

        http.formLogin() //�α��� ������ ���� ����
            .loginPage("/guest/login") //�α��� ������ URL ����
            .loginProcessingUrl("/guest/login_processing")
            // �α��� ���������� '�α���' ��ư�� �������� ��û�� URL ����
            .failureUrl("/guest/login?error")
            // �α����� �������� �� redirect URL ����
            .defaultSuccessUrl("/user/index", true)
            // �α����� �������� �� redirect URL ����
            .usernameParameter("loginId")
            .passwordParameter("passwd");
        	// �α��� ������(view)���� �α��� id input�±��� name���� ��й�ȣ input�±��� name�� ����

        http.logout() //�α׾ƿ� ���� ����
            .logoutRequestMatcher(new AntPathRequestMatcher("/user/logout_processing"))
            // �α׾ƿ� ��ư�̳� ��ũ�� ������ �� ��û�� URL ����
            .logoutSuccessUrl("/guest/index")
            //�α׾ƿ��� ��  redirect URL ����
            .invalidateHttpSession(true);
        	// �α׾ƿ��� �� session�� ����ִ� �����͸� ���� ������ ����

        http.authenticationProvider(myAuthenticationProvider);
    }
}
```

### 6. Controller Ŭ����

��ġ: src/main/java/net/lele/controller/GuestController.java

 - GuestController.java

```java
package net.lele.controller;

import javax.validation.Valid;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import net.lele.model.UserRegistrationModel;
import net.lele.service.UserService;

@Controller
public class GuestController {

	@Autowired
	UserService userService;
	
	@RequestMapping({ "/", "guest/index" })
	public String index() {
		return "guest/index";
	}

	@RequestMapping("guest/login")
	public String login() {
		return "guest/login";
	}

	@RequestMapping(value = "guest/register", method = RequestMethod.GET)
	public String register(UserRegistrationModel userModel, Model model) {
		return "guest/register";
	}

	@RequestMapping(value = "guest/register", method = RequestMethod.POST)
	public String register(@Valid UserRegistrationModel userModel, BindingResult bindingResult, Model model) {
		if (userService.hasErrors(userModel, bindingResult)) {
			return "guest/register";
		}
		userService.save(userModel);
		return "redirect:index";
	}
}
```

 - UserController

```java
package net.lele.controller

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class UserController {

    @RequestMapping("user/index")
    public String index() {
        return "user/index";
    }
}
```

 ��... ���� �⺻java������ �����Ű�����... view�� ������!! �ȴ���>_<��