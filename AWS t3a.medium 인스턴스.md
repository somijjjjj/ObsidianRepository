

[[2025-06-20]]OB맥주 AWS서버 인스턴스를 t3a.medium으로 사용

## AWS t3a.medium 인스턴스 개요

**t3a.medium** 인스턴스는 AWS EC2의 범용(burstable general purpose) 인스턴스 계열 중 하나로, AMD EPYC 프로세서를 기반으로 하며, 비용 효율성과 적당한 성능을 동시에 제공하는 것이 특징입니다[6](https://aws.amazon.com/ko/blogs/korea/now-available-amd-epyc-powered-amazon-ec2-t3a-instances/)[5](https://costcalc.cloudoptimo.com/aws-pricing-calculator/ec2/t3a.medium).

## 주요 사양

| 항목           | 사양 및 설명                             |
| ------------ | ----------------------------------- |
| vCPU         | 2개                                  |
| 메모리          | 4 GiB                               |
| 프로세서         | AMD EPYC 7000 시리즈 (예: 7571, 2.5GHz) |
| 네트워크         | 최대 5 Gbps                           |
| EBS 대역폭      | 최대 1.5 Gbps                         |
| 스토리지         | EBS 전용(로컬 디스크 없음)                   |
| 시간당 요금       | 약 $0.0376 (온디맨드, 리전별 상이)            |
| 아키텍처         | 64-bit                              |
| 가상화          | HVM                                 |
| Nitro System | 지원                                  |

## 특징 및 장점

- **버스트 가능한 성능**: t3a.medium은 기본적인 CPU 성능(베이스라인)을 제공하다가, 필요 시 CPU 크레딧을 사용해 일시적으로 높은 성능(버스팅)이 가능합니다[5](https://costcalc.cloudoptimo.com/aws-pricing-calculator/ec2/t3a.medium)[4](https://progdev.tistory.com/37)[6](https://aws.amazon.com/ko/blogs/korea/now-available-amd-epyc-powered-amazon-ec2-t3a-instances/).
    
- **무제한 모드 기본 적용**: 크레딧을 모두 소진해도 추가 비용을 내고 계속해서 버스팅이 가능합니다. 이 모드는 t3a 인스턴스에서 기본으로 활성화되어 있습니다[4](https://progdev.tistory.com/37)[5](https://costcalc.cloudoptimo.com/aws-pricing-calculator/ec2/t3a.medium).
    
- **비용 효율성**: 동일한 인텔 기반 t3.medium과 비교해 t3a.medium은 약 10% 저렴하며, t3 시리즈 대비 가성비가 우수합니다[6](https://aws.amazon.com/ko/blogs/korea/now-available-amd-epyc-powered-amazon-ec2-t3a-instances/)[4](https://progdev.tistory.com/37)[5](https://costcalc.cloudoptimo.com/aws-pricing-calculator/ec2/t3a.medium).
    
- **적합한 워크로드**: CPU 사용량이 대부분 낮거나, 간헐적으로만 높은 부하가 발생하는 웹 서버, 개발/테스트 환경, 소규모 데이터베이스 등에 적합합니다[6](https://aws.amazon.com/ko/blogs/korea/now-available-amd-epyc-powered-amazon-ec2-t3a-instances/)[4](https://progdev.tistory.com/37).
    

## t3.medium vs t3a.medium

|항목|t3.medium (Intel)|t3a.medium (AMD)|
|---|---|---|
|CPU|Intel Xeon|AMD EPYC|
|vCPU|2|2|
|메모리|4 GiB|4 GiB|
|가격|다소 높음|약 10% 저렴|
|성능 차이|거의 없음|거의 없음|

t3a.medium은 t3.medium과 사양이 거의 동일하지만, AMD CPU를 사용해 가격이 더 저렴합니다. 성능 차이는 실사용에서 거의 느껴지지 않으므로, 비용을 중요시한다면 t3a.medium이 더 효율적입니다[4](https://progdev.tistory.com/37)[6](https://aws.amazon.com/ko/blogs/korea/now-available-amd-epyc-powered-amazon-ec2-t3a-instances/).

## 사용 시 주의점

- **지속적 고성능 요구 시 비효율**: 24시간 내내 CPU를 많이 사용하는 워크로드에는 적합하지 않으며, 이런 경우 m5.large 등 고정 성능 인스턴스가 더 적합합니다[4](https://progdev.tistory.com/37)[7](https://devmuromi.tistory.com/entry/EC2-T-instance-VS-M-instance-%EB%B9%84%EC%9A%A9-%EB%B9%84%EA%B5%90).
    
- **CPU 크레딧 관리 필요**: 버스팅 성능은 CPU 크레딧에 따라 제한될 수 있으므로, CloudWatch 등으로 크레딧 사용량을 모니터링하는 것이 좋습니다[4](https://progdev.tistory.com/37).
    

## 요약

t3a.medium 인스턴스는 2 vCPU, 4 GiB 메모리, AMD EPYC 기반의 저렴한 가격, 버스트 가능한 CPU 성능이 특징인 범용 인스턴스입니다. 평소에는 CPU 사용량이 낮고, 간헐적으로만 높은 부하가 발생하는 서비스에 매우 적합하며, 가성비가 뛰어난 선택지입니다[5](https://costcalc.cloudoptimo.com/aws-pricing-calculator/ec2/t3a.medium)[6](https://aws.amazon.com/ko/blogs/korea/now-available-amd-epyc-powered-amazon-ec2-t3a-instances/)[4](https://progdev.tistory.com/37).

1. [https://aws.amazon.com/ko/ec2/instance-types/t3/](https://aws.amazon.com/ko/ec2/instance-types/t3/)
2. [https://aws.amazon.com/ko/ec2/instance-types/](https://aws.amazon.com/ko/ec2/instance-types/)
3. [https://devocean.sk.com/blog/techBoardDetail.do?ID=163854](https://devocean.sk.com/blog/techBoardDetail.do?ID=163854)
4. [https://progdev.tistory.com/37](https://progdev.tistory.com/37)
5. [https://costcalc.cloudoptimo.com/aws-pricing-calculator/ec2/t3a.medium](https://costcalc.cloudoptimo.com/aws-pricing-calculator/ec2/t3a.medium)
6. [https://aws.amazon.com/ko/blogs/korea/now-available-amd-epyc-powered-amazon-ec2-t3a-instances/](https://aws.amazon.com/ko/blogs/korea/now-available-amd-epyc-powered-amazon-ec2-t3a-instances/)
7. [https://devmuromi.tistory.com/entry/EC2-T-instance-VS-M-instance-%EB%B9%84%EC%9A%A9-%EB%B9%84%EA%B5%90](https://devmuromi.tistory.com/entry/EC2-T-instance-VS-M-instance-%EB%B9%84%EC%9A%A9-%EB%B9%84%EA%B5%90)
8. [https://docs.aws.amazon.com/ko_kr/ec2/latest/instancetypes/ec2-types.pdf](https://docs.aws.amazon.com/ko_kr/ec2/latest/instancetypes/ec2-types.pdf)
9. [https://instances.vantage.sh/aws/ec2/t3a.medium](https://instances.vantage.sh/aws/ec2/t3a.medium)
10. [https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/cpu-options-supported-instances-values.html](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/cpu-options-supported-instances-values.html)
11. [https://www.reddit.com/r/aws/comments/9pwx5d/unclear_on_benefits_of_t_series_vs_m_series/?tl=ko](https://www.reddit.com/r/aws/comments/9pwx5d/unclear_on_benefits_of_t_series_vs_m_series/?tl=ko)
12. [https://spidyweb.tistory.com/311](https://spidyweb.tistory.com/311)
13. [https://imprint.tistory.com/102](https://imprint.tistory.com/102)
14. [https://engmisankim.tistory.com/28](https://engmisankim.tistory.com/28)


---


## t3a.medium이 적합한 워크로드 유형

**t3a.medium** 인스턴스는 범용(burstable general purpose) 인스턴스 계열로, 다음과 같은 워크로드에 특히 적합합니다:

- **가벼운 웹 서버**: 트래픽이 일정하지 않고, 대부분의 시간에는 낮은 CPU 사용률을 보이지만 간헐적으로 요청이 몰릴 수 있는 웹 서비스[5](https://idea9329.tistory.com/279)[6](https://choiblog.tistory.com/220).
    
- **개발 및 테스트 환경**: 개발자용 서버, CI/CD 빌드 서버, 테스트 자동화 환경 등 일시적으로만 리소스를 많이 사용하는 환경[5](https://idea9329.tistory.com/279).
    
- **저사양 데이터베이스**: 소규모 데이터베이스, 캐시 서버 등 CPU 사용량이 낮고 메모리 요구사항이 크지 않은 데이터베이스 서버[5](https://idea9329.tistory.com/279).
    
- **간헐적으로 트래픽이 변동하는 애플리케이션**: 주기적으로만 높은 부하가 발생하는 API 서버, 소규모 백엔드 서비스 등[1](https://devocean.sk.com/blog/techBoardDetail.do?ID=163854)[5](https://idea9329.tistory.com/279).
    
- **코드 리포지토리, 내부 툴 서버**: Git, 사내 Wiki, 경량화된 업무용 도구 등[4](https://aws.amazon.com/ko/ec2/instance-types/)[6](https://choiblog.tistory.com/220).
    

## 특징 및 장점

- **버스트 가능한 CPU 성능**: 대부분의 시간에는 낮은 CPU 성능을 사용하다가, 필요할 때만 일시적으로 높은 성능을 낼 수 있어 비용 효율적입니다[1](https://devocean.sk.com/blog/techBoardDetail.do?ID=163854)[5](https://idea9329.tistory.com/279).
    
- **비용 효율성**: 고정적으로 높은 성능이 필요하지 않은 워크로드에 대해 저렴한 비용으로 충분한 성능을 제공합니다[2](https://aws.amazon.com/ko/ec2/instance-types/t3/)[5](https://idea9329.tistory.com/279).
    
- **균형 잡힌 리소스**: CPU와 메모리의 비율이 균형 잡혀 있어 다양한 범용 애플리케이션에 사용할 수 있습니다[6](https://choiblog.tistory.com/220).
    

## 주의해야 할 점

- **지속적으로 높은 CPU 성능이 필요한 워크로드에는 부적합**: 24시간 내내 CPU를 많이 사용하는 작업(예: 대규모 데이터 처리, 머신러닝 트레이닝 등)에는 적합하지 않습니다. 이런 경우에는 C 시리즈(컴퓨팅 최적화)나 M 시리즈(고정 성능 범용) 인스턴스를 고려해야 합니다[5](https://idea9329.tistory.com/279)[6](https://choiblog.tistory.com/220).
    

---

**요약:**  
t3a.medium은 CPU 사용량이 대부분 낮고, 간헐적으로만 높은 부하가 발생하는 웹 서버, 개발/테스트 환경, 소규모 데이터베이스, 내부 툴 서버 등 비용 효율성과 유연성이 중요한 워크로드에 적합합니다[1](https://devocean.sk.com/blog/techBoardDetail.do?ID=163854)[5](https://idea9329.tistory.com/279)[6](https://choiblog.tistory.com/220).

1. [https://devocean.sk.com/blog/techBoardDetail.do?ID=163854](https://devocean.sk.com/blog/techBoardDetail.do?ID=163854)
2. [https://aws.amazon.com/ko/ec2/instance-types/t3/](https://aws.amazon.com/ko/ec2/instance-types/t3/)
3. [https://blog.naver.com/techtrip/222055595568](https://blog.naver.com/techtrip/222055595568)
4. [https://aws.amazon.com/ko/ec2/instance-types/](https://aws.amazon.com/ko/ec2/instance-types/)
5. [https://idea9329.tistory.com/279](https://idea9329.tistory.com/279)
6. [https://choiblog.tistory.com/220](https://choiblog.tistory.com/220)
7. [https://dev.classmethod.jp/articles/how-to-select-the-ec2-instance-type-kr/](https://dev.classmethod.jp/articles/how-to-select-the-ec2-instance-type-kr/)
8. [https://velog.io/@repush/Amazon-EC2%EC%9D%98-%EC%A2%85%EB%A5%98%EC%99%80-%EA%B8%B0%EB%8A%A5](https://velog.io/@repush/Amazon-EC2%EC%9D%98-%EC%A2%85%EB%A5%98%EC%99%80-%EA%B8%B0%EB%8A%A5)


