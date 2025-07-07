
# AWS VPC 구성 리소스 요약 (QueryPie 환경)

이 문서는 제공된 아키텍처 다이어그램 기반으로 AWS VPC 내 리소스들을 카테고리별로 정리한 표입니다.


## 📦 VPC 및 서브넷

| 리소스 이름         | 설명                    |
|---------------------|-------------------------|
| Test망 VPC          | 주요 VPC (Private 네트워크) |
| Private Subnet 1개  | QueryPie EC2가 배치된 서브넷 |
| Public Subnet 1개   | NAT Gateway 포함 서브넷 |

## 🖥️ 컴퓨트 (EC2)

| 리소스 이름      | 설명 |
|------------------|------|
| QueryPie EC2     | m5.large, Amazon Linux 2023 |
| QueryPie EC2 SG  | 보안 그룹 (포트 80, 443, 9000, 9022 허용) |

## ⚙️ 네트워크 Load Balancer (NLB)

| NLB 종류   | 설명                          |
|------------|-------------------------------|
| RTA용 NLB | 9022 포트 인바운드 허용 |
| 접속용 NLB | 80, 443, 9000 포트 인바운드 허용 |

| 보안 그룹 이름         | 인바운드 포트       | 아웃바운드 |
|------------------------|----------------------|------------|
| RTA용 NLB SG           | 9022 all             | all        |
| 접속용 NLB SG          | 80, 443, 9000 all    | all        |

## 🔒 보안 및 인증

| 리소스 이름          | 설명 |
|-----------------------|------|
| VPC Endpoint Service  | RTA용 NLB에 연결됨 |
| Route 53 Domain       | 접속용 NLB에 연결됨 |
| ACM 인증서            | Route 53 도메인 연결 인증서 |

## 🌐 라우팅 및 게이트웨이

| 리소스 이름         | 설명 |
|----------------------|------|
| Route Table          | 0.0.0.0/0 → NAT, local → local |
| NAT Gateway          | 인터넷 접속용 (Private Subnet용) |
| Internet Gateway     | Public Subnet용 |

