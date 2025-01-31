# 6장 데이터베이스 연동


## 6.1 마리아DB 설치


## 6.2 ORM
##### ORM 장/단점


## 6.3 JPA


## 6.4 하이버네이트
### 6.4.1 Spring Data JPA


## 6.5 영속성 컨텍스트
### 6.5.1 엔티티 매니저

### 6.5.2 엔티티의 생명주기


## 6.6 데이터 베이스 연동

### 6.6.1 프로젝트 생성
##### 옵션 설명


## 6.7 엔티티 설계
### 6.7.1 엔티티 관련 기본 어노테이션


## 6.8 리포지토리 인터페이스 설계
### 6.8.1 리포지토리 인터페이스 생성

### 6.8.2 리포지토리 메서드의 생성 규칙


## 6.9 DAO 설계
### DAO(Data Access Object)
     데이터베이스에 접근(조회/수정)하기 위한 로직을 관리하기 위한 객체.
    
#### DAO를 사용하는 이유(장점)
* 비즈니스 로직과 데이터 액세스 로직 분리
* 변경에 대한 유연성(유지보수 용이)
* 재사용성
* 테스트 용이성
#### DAO의 단점
* 복잡성 증가
* 성능 문제

#### DAO의 포함되어야 하는 구성요소(기능)
1. DB연결
2. 데이터의 CRUD(Create, Read, Update, Delete)
3. 데이터 매핑(DTO)

### 6.9.1 DAO 클래스 생성
####  ProductDAO 인터페이스 
* 데이터 접근을 위한 메서드의 시그니처들 정의한 인터페이스
```java
package com.springboot.jpa.data.dao;

import com.springboot.jpa.data.entity.Product;

public interface ProductDAO {
    Product insertProduct(Product product);
    Product selectProduct(Long number);
    Product updateProductName(Long number, String name) throws Exception;
    void deleteProduct(Long number) throws Exception;
}
```
####  ProductDAO 인터페이스의 구현체 클래스
```java
package com.springboot.jpa.data.dao.impl;

import org.springframework.stereotype.Component;
import org.springframework.beans.factory.annotation.Autowired;

@Component
public class ProductDAOImpl implements ProductDAO {

    private final ProductRepository productRepository;

    @Autowired
    public ProductDAOImpl(ProductRepository productRepository) {
        this.productRepository = productRepository;
    }

    @Override
    public Product insertProduct(Product product) {
        return null;
    }
    public Product selectProduct(Long number) {
    return null;


    @Override
    public Product updateProductName(Long number, String name) throws Exception {
    return null;
    }

    @Override
    public void deleteProduct(Long number) throws Exception {
    }
}
```
#### insertProduct() 메서드
```java
@Override
public Product insertProduct(Product product) { 
    Product savedProduct = productRepository.save(product);
     
    return savedProduct;
}
```
#### selectProduct() 메서드
```java
@Override 
public Product selectProduct(Long number){ 
    Product selectedProduct = productRepository.getByid(number); 
    
    return selectedProduct; 
}
```
#### SimpleJpaRepository의 getByld() 메서드
```java
@Override
public T getById(ID id) {
    Assert.notNull(id, "ID_MUST_NOT_BE_NULL");
    return em.getReference(getDomainClass(), id);
}
```
####  SimpleJpaRepository의 findByld() 메서드
```java
@Override
public Optional<T> findById(ID id) {

    Assert.notNull(id, "ID_MUST_NOT_BE_NULL");

    Class<T> domainType = getDomainClass();

    if (metadata == null) {
        return Optional.ofNullable(em.find(domainType, id));
    }

    LockModeType type = metadata.getLockModeType();
    
    Map<String, Object> hints = new HashMap<>();
    getQueryHints().withFetchGraphs(em).forEach(hints::put);

    return Optional.ofNullable(type == null 
        ? em.find(domainType, id, hints) 
        : em.find(domainType, id, type, hints));
}
```
#### updateProductName() 메서드
```java
@Override
public Product updateProductName(Long number, String name) throws Exception {
    Optional<Product> selectedProduct = productRepository.findById(number);
    
    Product updatedProduct;
    if (selectedProduct.isPresent()) {
        Product product = selectedProduct.get();
        
        product.setName(name);
        product.setUpdatedAt(LocalDateTime.now());
        
        updatedProduct = productRepository.save(product);
    } 
    else {
        throw new Exception();
    }

    return updatedProduct;
}
```
#### SimpleJpaRepository의 save() 메서드
```java
@Transactional
@Override
public <S extends T> S save(S entity) {

    Assert.notNull(entity, "Entity must not be null.");

    if (entityInformation.isNew(entity)) {
        em.persist(entity);
        return entity;
    } 
    else {
        return em.merge(entity);
    }
}
```
#### deleteProduct() 메서드
```java
@Override
public void deleteProduct(Long number) throws Exception {
    Optional<Product> selectedProduct = productRepository.findById(number);

    if (selectedProduct.isPresent()) {
        Product product = selectedProduct.get();
        
        productRepository.delete(product);
    } 
    else {
        throw new Exception();
    }
}
```
#### SimpleJpaRepository의 delete() 메서드
```java
@Override
@Transactional
@SuppressWarnings("unchecked")
public void delete(T entity) {

    Assert.notNull(entity, "Entity must not be null!");

    if (entityInformation.isNew(entity)) {
        return;
    }

    Class<?> type = ProxyUtils.getUserClass(entity);
    T existing = (T) em.find(type, entityInformation.getId(entity));

    // If the entity to be deleted doesn't exist, delete is a NOOP
    if (existing == null) {
        return;
    }

    em.remove(em.contains(entity) ? entity : em.merge(entity));
}
```
## DAO 연동을 위한 컨트롤러와 서비스 설계
### 6.10.1 서비스 클래스 만들기
#### ProductDto 클래스
```java
public class ProductDto { 

    private String name; 
    private int price; 
    private int stock; 

    public ProductDto(String name, int price, int stock) { 
        this.name = name; 
        this.price = price; 
        this.stock = stock; 
    }

    public String getName() { 
        return name; 
    }

    public void setName(String name) { 
        this.name = name; 
    }

    public int getPrice() { 
        return price; 
    }

    public void setPrice(int price) { 
        this.price = price; 
    }

    public int getStock() { 
        return stock; 
    }

    public void setStock(int stock) { 
        this.stock = stock; 
    }
}
```
#### ProductResponseDto 클래스
```java
public class ProductResponseDto { 

    private Long number; 
    private String name; 
    private int price; 
    private int stock; 

    public ProductResponseDto() {}

    public ProductResponseDto(Long number, String name, int price, int stock) { 
        this.number = number; 
        this.name = name; 
        this.price = price; 
        this.stock = stock; 
    }

    public Long getNumber() { 
        return number; 
    }

    public void setNumber(Long number) { 
        this.number = number; 
    }

    public String getName() { 
        return name; 
    }

    public void setName(String name) { 
        this.name = name; 
    }

    public int getPrice() { 
        return price; 
    }

    public void setPrice(int price) { 
        this.price = price; 
    }

    public int getStock() { 
        return stock; 
    }

    public void setStock(int stock) { 
        this.stock = stock; 
    }
}
```
#### ProductService 인터페이스 설계
```java
public interface ProductService { 

    ProductResponseDto getProduct(Long number); 

    ProductResponseDto saveProduct(ProductDto productDto);

    ProductResponseDto changeProductName(Long number, String name) throws Exception;

    void deleteProduct(Long number) throws Exception;
}
```

![structure](https://github.com/MT-H-B/study-summaries/blob/main/structure.png)
