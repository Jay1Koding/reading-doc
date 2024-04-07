# JPA = Entity + JpaRepository

> 전반적인 프로세스
>
> 1. Entity ID GeneratedValue 전략 설정
> 2. JpaRepository<Entity, IdType>
> 3. repository.save()

```java
public interface CrudRepository<T, ID> extends JpaRepository<T, ID> {

  <S extends T> S save(S entity);

  Optional<T> findById(ID primaryKey);

  Iterable<T> findAll();

  long count(); // 엔티티 수를 반환

  void delete(T entity);

  boolean existsById(ID primaryKey);

  // … more functionality omitted.
}

// pagination을 위한 추가 메소드
public interface PagingAndSortingRepository<T, ID>  {

  Iterable<T> findAll(Sort sort);

  Page<T> findAll(Pageable pageable);
}

PagingAndSortingRepository<User, Long> repository = // … get access to a bean
Page<User> users = repository.findAll(PageRequest.of(1, 20));
```
