---
title: 길고양이 연구 - 2. 구현
layout: post
tags: [개발]
---

1. [길고양이 연구 - 1. 문제](2020-05-04-street-cat-study-1.md)

## API

### 새 인식표 등록하기

1. 인식표와 장비 정보를 서버로 보내면
2. 등록된 정보가 있는지 확인한다.
    1. 없으면 새로 등록한다.
3. 등록된 인식표 정보를 반환한다.

요약하면, "cats 테이블에 데이터를 저장하기"가 된다.

![cats 테이블]({{ base.url }}/assets/street-cat-study/2_table_cats.png)

테이블에 저장할 수 있도록 JPA 엔티티 매핑을 한다.
```java
@Entity(name = "Cat")
@Table(name = "cats",
    uniqueConstraints = @UniqueConstraint(name = "uq_cats", columnNames = {"chip_id", "device_id"}))
public class Cat {
  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  private long id;
  @Column(name = "chip_id", nullable = false, updatable = false)
  private UUID chipId;
  @Column(name = "device_id", nullable = false, updatable = false)
  private UUID deviceId;
  @Column(name = "memo")
  private String memo;
  @Column(name = "created_at", nullable = false, updatable = false)
  private LocalDateTime createdAt;
  @Column(name = "updated_at", nullable = false)
  private LocalDateTime updatedAt;
  @Column(name = "deleted_at")
  private LocalDateTime deletedAt;

  public Cat() {
  }

  public Cat(UUID chipId, UUID deviceId) {
    this.chipId = chipId;
    this.deviceId = deviceId;
  }

  // ... 생략 ...
}
```

저장을 해야 하는 상황인지, 기존의 데이터를 돌려줘야 하는지 판단하고 실행하는 로직.
```java
@Service
@Transactional
public class CatService {
  @Autowired
  private CatRepository catRepository;

  public Cat add(UUID chipId, UUID deviceId) {
    Cat cat = this.catRepository.findByChipIdAndDeviceId(chipId, deviceId);
    if (null == cat) {
      cat = this.catRepository.save(new Cat(chipId, deviceId));
    }
    return cat;
  }
}
```

## 참고

- https://github.com/JustBurrow/pages/street-cat-study : 예제 코드