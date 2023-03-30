---
layout: single
title:  "서울시 구별 CCTV 현황 분석하기"
categories: MachineLearning
toc: true
---

# csv 파일 읽기 - 서울시 구별 CCTV 현황

```python
CCTV_Seoul = pd.read_csv('CCTV_in_Seoul.csv')
CCTV_Seoul.head(3)
CCTV_Seoul.rename(columns={'기관명':'구'}, inplace=True)
CCTV_Seoul
```