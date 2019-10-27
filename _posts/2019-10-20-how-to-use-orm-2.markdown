---
title: 웹 디자이너가 템플릿을 바로 작업할 수 있게 해줬더니 DB가 죽었어요 (2)
layout: post
tags: [orm, modeling]
---

> 각 분류에는 여러 개의 상품을 목록으로 보여준다.

이 기능을 만들어보자. 다음이 필요하다.

1. 분류(`Category`)의 이름과 배경 이미지 등 메타 정보.
1. 분류에 해당하는 상품(`Product`) 목록을 출력하기 위한 ID, 이름 등 메타 정보.

로직은 간단하다. 하지만 사용하는 기술(OM과 ORM)에 따라 적용 가능한 개념(Procedural과 OOP)이 달라지고, 코드 재사용성이 달라진다.
더 나아가 유지보수 비용(기술적 부채)가 달라지고, 최종적으로 소프트웨어의 수명이 달라진다.

그동안 개발자의 직업 수명이 달라지고, 인수인계 비용이 발생하는 건 덤이다.

## OM의 경우

1. `CategoryController`에 분류 ID를 전달한다.
1. `CategoryService`에 분류 정보를 요청한다.
1. `ProductService`에 분류에 해당하는 상품 목록을 요청한다.
1. `CategoryController`가 `Model`에 분류와 상품 목록을 추가하고, 템플릿 이름을 반환한다.
1. 템플릿이 분류(`category`)와 상품(`products`) 정보를 출력한다.

![OM category page sequence diagram]({{ base.url }}/assets/how-to-use-orm/om_category_page_sequence_diagram.jpg)

```java
@Controller
public class CategoryController {
  @Autowired
  private CategoryService categoryService;
  @Autowired
  private ProductService productService;

  @GetMapping("/category/{id:[1-9]\\d*}")
  public String read(@PathVariable("id") final long id, final Model model) {
    Category category = this.categoryService.read(id);
    List<Product> products = this.productService.products(id);  // OM에서 product 테이블을 읽는 시점.

    model.addAttribute("category", category);
    model.addAttribute("products", products);

    return "page/category";
  }
}
```
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" th:lang="${#locale}">
<head>
    <title th:text="${category.name}">Category</title>
</head>
<body>
<main>
    <h1 th:text="${category.name}">Category</h1>
    <section>
        <h2>Products</h2>
        <table>
            <caption>Products</caption>
            <thead><tr><th><span>ID</span></th><th><span>NAME</span></th></tr></thead>
            <tbody>
            <tr th:each="product : ${products}">
                <td><span th:text="${product.id}">id</span></td>
                <td><span th:text="${product.name}">name</span></td>
            </tr>
            </tbody>
        </table>
    </section>
</main>
</body>
</html>
```

## ORM의 경우

1. `CategoryController`에 분류 ID를 전달한다.
1. `CategoryService`에 분류 정보를 요청한다.
1. `ProductService`에 분류에 해당하는 상품 목록을 요청한다.
1. `CategoryController`가 `Model`에 분류를 추가하고, 템플릿 이름을 반환한다.
1. 템플릿이 분류(`category`)를 출력하고, 분류에서 해당 상품 목록을 가져와서 출력한다.

![ORM category page sequence diagram]({{ base.url }}/assets/how-to-use-orm/orm_category_page_sequence_diagram.jpg)

```java
@Controller
public class CategoryController {
  @Autowired
  private CategoryService categoryService;

  @GetMapping("/category/{id:[1-9]\\d*}")
  public String read(@PathVariable("id") final long id, final Model model) {
    Category category = this.categoryService.read(id);
    model.addAttribute("category", category);
    return "page/category";
  }
}
```
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" th:lang="${#locale}">
<head>
    <title th:text="${category.name}">category</title>
</head>
<body>
<main>
    <h1 th:text="${category.name}">category</h1>
    <section>
        <h2>Products</h2>
        <table>
            <caption>Products</caption>
            <thead>
            <tr><th><span>ID</span></th><th><span>NAME</span></th></tr>
            </thead>
            <tbody>
            <tr th:each="product : ${category.products}"><!-- ORM에서 product 테이블을 읽는 시점. -->
                <td>
                    <span th:text="${product.id}">id</span>
                </td>
                <td>
                    <span th:text="${product.name}">name</span>
                </td>
            </tr>
            </tbody>
        </table>
    </section>
</main>
</body>
</html>
```

## 비교

|OM(MyBatis)|ORM(JPA/Hibernate)|
|--|---|
|![OM category page sequence diagram]({{ base.url }}/assets/how-to-use-orm/om_category_page_sequence_diagram.jpg)|![ORM category page sequence diagram]({{ base.url }}/assets/how-to-use-orm/orm_category_page_sequence_diagram.jpg)|

분류(`Category`)를 읽을 때까지는 동일하지만 이후의 코드가 다르다.

- OM은 제품목록을 읽는 로직이 `CategoryService`와 `ProductService`를 사용할 수 있는 컨트롤러(`CategoryController.read(long, Model)`)에 있다.
- ORM은 제품목록을 읽는 로직이 분류(`Category`) 자신이 가지고(`getProducts()` 메서드) 있다.

이 차이 때문에 OM은 `CategoryController`에 접근할 수 있어야만 카테고리의 제품 목록을 알 수 있지만, ORM은 `Category`에 접근할 수 있으면 어떤 로직에서도 제품 목록을 알아낼 수 있다.

## 기타

`MyBatis`의 result set을 사용하면 분류를 읽을 때 제품목록도 함께 읽을 수 있다.
하지만 제품 목록이 필요 없을 경우에도 항상 제품 목록을 읽기 때문에 SQL 쿼리 최적화를 할 수 없게 된다.