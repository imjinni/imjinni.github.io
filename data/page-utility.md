---
layout: single
title: "포스트 페이지 기능 모음"
toc-top: true
---
## 포스트에서 독백

<div class="md-monologue" markdown=1>
독백 내용을 쓸 수 있음
</div>

## 이미지 밑에 보충 설명이나 출처

![CNN Structure]({{ site.gdrive_url_prefix }}1N5DRVwVs4MtD2RLz2N6BwaINFyFcbbFb)
{:style="margin-bottom: 0;" class="img-popup" data-title="특별 출연한 귀여운 고양이의 이름은 <strong>까무</strong>입니다 😆"}
<div style="font-size: .75em;" markdown=1>
특별 출연한 귀여운 고양이의 이름은 <strong>까무</strong>입니다 😆
</div>

## 이미지 Magnific-Popup과 타이틀

(클릭해보기)<br/>
![CNN Structure]({{ site.gdrive_url_prefix }}1N5DRVwVs4MtD2RLz2N6BwaINFyFcbbFb)
{:class="img-popup" data-title="특별 출연한 귀여운 고양이의 이름은 <strong>까무</strong>입니다 😆"}

## 포스트 맨 밑에 Reference

어쩌구 저쩌구 포스트 내용 끝

<div class="md-reference" markdown=1>
* <https://towardsdatascience.com/a-comprehensive-introduction-to-different-types-of-convolutions-in-deep-learning-669281e58215#:~:text=A%20%E2%80%9CKernel%E2%80%9D%20refers%20to%20a,is%20a%20collection%20of%20kernels.>
* <http://taewan.kim/post/cnn/>
</div>

## Markdown Alias Link

글 중에 아래처럼 링크 넣고

![easy insert measurement id to config in mmistakes theme][1]

포스트 맨 밑에 아래 처럼 링크 Alias 내용 기록

[1]: {{ site.gdrive_url_prefix }}1MqYY0G1PRf1BS5xozVyv0P9K-mFvV37O

## Markdown Footnote 주석

* 이렇게 쓰고 `masthead`[^masthead] 포스트 맨아래에 주석 내용 입력

[^masthead]: masthead: 돛대의 꼭대기, 신문 1면의 발행사 이름을 의미한다. 즉, Topbar와 크게 다르지 않은 의미이다.

## 코드 블록 접기/펴기

이렇게 하면 접고 펴기 버튼을 달 수 있고(`code-reducible`)<br/>
초기 상태로 접어 놓을지 펴 놓을지 지정할 수 있음(`code-reduce`)

<div class="code-reducible code-reduce" markdown="1">
```
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
```
</div>

## 특정 조건에서만 작동하는 JS

아래와 같이 감춰진 메모 클래스를 selector로 놓고 `data-type` 값에 따라 조건 분기 하기

<div class="memo" hidden data-type="toc"></div>