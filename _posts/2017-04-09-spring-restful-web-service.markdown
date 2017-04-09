---
layout: post
title: RESTful Web Service
---

대단한 것은 아니고, `@PatchMapping`, `@DeleteMapping`등의 HTTP method를 사용하는 방법.
`org.springframework.web.filter.HiddenHttpMethodFilter` 인스턴스를 만들어서 빈으로 추가한다.

## 컨트롤러

```java
package com.app.editor.web.controller;

import com.app.editor.web.controller.req.CreateSiteReq;
import com.app.editor.web.controller.req.UpdateSiteReq;
import com.app.editor.web.exception.HttpException;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.*;

import javax.validation.Valid;

/**
 * @author justburrow
 * @since 2017. 4. 3.
 */
@RequestMapping("/sites")
public interface SiteController {
  /**
   * @param req
   * @param result
   * @param model
   * @return
   * @throws HttpException
   */
  @PatchMapping("/{id:[1-9]\\d*}")
  String update(int id, UpdateSiteReq req, BindingResult result, Model model) throws HttpException;
}
```

## 설정

```java
package com.app.editor.web.configuration;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.filter.HiddenHttpMethodFilter;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;

/**
 * @author justburrow
 * @since 2017. 4. 2.
 */
@Configuration
@EnableWebMvc
public class WebMvcConfiguration extends WebMvcConfigurerAdapter {
  @Bean
  public HiddenHttpMethodFilter hiddenHttpMethodFilter() {
    return new HiddenHttpMethodFilter();
  }
}
```
