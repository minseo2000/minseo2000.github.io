---
author: 박민서
date: 2026-01-26
category: 배터리설치2
layout: post
mermaid: true
---

<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>서울대학교 병원 UPS 배터리 설치 완료!</title>
  <style>
    body {
      font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
      line-height: 1.6;
      margin: 0;
      padding: 20px;
      background-color: #f5f5f5;
      color: #333;
    }
    .container {
      max-width: 800px;
      margin: 0 auto;
      background-color: white;
      padding: 30px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }
    h1 {
      color: #2c3e50;
      text-align: center;
      border-bottom: 2px solid #3498db;
      padding-bottom: 10px;
    }
    .meta-info {
      text-align: center;
      color: #7f8c8d;
      margin-bottom: 30px;
      font-size: 0.9em;
    }
    :root {
      --gallery-ratio: 4 / 3;
      --img-radius: 10px;

      /* ✅ 연락처(내사진/명함) 행 높이: 여기만 바꾸면 크기 조절 */
      --contact-row-h: 170px;
    }

    /* =========================
       갤러리
    ========================= */
    .photo-gallery {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 15px;
      margin: 30px 0;
    }
    .photo-item img {
      width: 100%;
      aspect-ratio: var(--gallery-ratio);
      object-fit: cover;
      border-radius: 8px;
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
      background: #fff;
      display: block;
    }
    .photo-caption {
      margin-top: 8px;
      font-size: 0.85em;
      color: #7f8c8d;
      text-align: center;
    }
    .photo-item.full-width {
      grid-column: 1 / -1;
    }
    .photo-item.full-width img {
      aspect-ratio: 16 / 9;
    }

    /* =========================
       ✅ 블로그 글 영역
       (설치 장소 사진과 담당자/연락처 사이)
       ✅ 글씨 "더" 크게 조정 완료
    ========================= */
    .post-section {
      margin: 34px 0 10px;
      padding: 22px;
      border: 1px solid #e5e7eb;
      border-radius: 14px;
      background: #ffffff;
    }
    .post-title {
      margin: 0 0 14px;
      font-size: 1.75rem; /* ✅ 더 크게 */
      font-weight: 800;
      color: #2c3e50;
      border-left: 4px solid #3498db;
      padding-left: 10px;
    }
    .post-body {
      color: #333;
      line-height: 1.95;  /* ✅ 더 넉넉하게 */
      font-size: 1.35rem; /* ✅ 더 크게 */
    }
    .post-body p {
      margin: 0 0 14px;
    }
    .post-body ul,
    .post-body ol {
      margin: 0 0 14px 26px; /* ✅ 들여쓰기 조금 증가 */
      padding: 0;
    }
    .post-body li {
      margin: 8px 0;
    }
    .post-body .info-box {
      margin-top: 16px;
      padding: 16px 18px;
      background: #f8fafc;
      border: 1px solid #e5e7eb;
      border-radius: 12px;
      font-size: 1.25rem; /* ✅ 박스 글씨도 더 크게 */
    }
    .post-body .info-box strong {
      color: #2c3e50;
    }

    /* =========================
       담당자/연락처 섹션
       ✅ 핵심: 가로 1:3, 높이 고정
    ========================= */
    .contact-section {
      margin-top: 40px;
      padding: 26px;
      border: 1px solid #e5e7eb;
      border-radius: 14px;
      background: #fafafa;
    }
    .contact-title {
      font-weight: 700;
      color: #2c3e50;
      margin-bottom: 16px;
    }

    .contact-grid {
      display: grid;
      grid-template-columns: 1fr 3fr; /* ✅ 무조건 1:3 */
      gap: 18px;
      align-items: stretch;
    }

    .card-box {
      border: 1px dashed #cbd5e1;
      border-radius: 14px;
      background: #fff;

      height: var(--contact-row-h); /* ✅ 두 박스 높이 동일 */
      padding: 12px;
      box-sizing: border-box;

      display: block;  /* flex 제거 */
      min-height: 0;   /* ✅ 기존 min-height 영향 제거 */
    }

    .profile-photo,
    .business-card-img {
      width: 100%;
      height: 100%; /* ✅ card-box 높이를 그대로 사용 */
      border-radius: var(--img-radius);
      overflow: hidden;
      border: 1px solid #e5e7eb;
      background: #fff;
      box-sizing: border-box;
    }

    /* 내 사진: 꽉 차게 */
    .profile-photo img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      display: block;
    }

    /* 명함: 전체가 다 보이게 */
    .business-card-img {
      cursor: zoom-in;
    }
    .business-card-img img {
      width: 100%;
      height: 100%;
      object-fit: contain;
      padding: 10px;
      background: #fff;
      display: block;
      box-sizing: border-box;
    }

    /* =========================
       라이트박스
    ========================= */
    .lightbox {
      position: fixed;
      inset: 0;
      background: rgba(0, 0, 0, 0.65);
      display: none;
      align-items: center;
      justify-content: center;
      z-index: 9999;
      padding: 20px;
    }
    .lightbox.open {
      display: flex;
    }
    .lightbox-content {
      max-width: 960px;
      width: 96vw;
      background: #fff;
      border-radius: 14px;
      overflow: hidden;
      position: relative;
    }
    .lightbox-content img {
      width: 100%;
      display: block;
    }
    .lightbox-close {
      position: absolute;
      top: 10px;
      right: 10px;
      background: rgba(0, 0, 0, 0.6);
      color: #fff;
      border: none;
      width: 36px;
      height: 36px;
      border-radius: 50%;
      cursor: pointer;
      font-size: 18px;
    }

    .footer {
      text-align: center;
      margin-top: 30px;
      padding-top: 20px;
      border-top: 1px solid #eee;
      color: #95a5a6;
      font-size: 0.8em;
    }

    /* =========================
       반응형
    ========================= */
    @media (max-width: 600px) {
      .photo-gallery {
        grid-template-columns: 1fr;
      }

      /* 모바일은 세로로 쌓기 */
      .contact-grid {
        grid-template-columns: 1fr;
      }

      /* 모바일에서 보기 좋게 높이 조정 */
      .card-box {
        height: 200px;
      }

      /* ✅ 모바일에서도 큼직하게 유지 */
      .post-title { font-size: 1.55rem; }
      .post-body  { font-size: 1.25rem; line-height: 1.9; }
      .post-body .info-box { font-size: 1.18rem; }
    }
  </style>
</head>

<body>
  <div class="container">
    <h1>서울대학교 병원 UPS 배터리 설치 완료!</h1>

    <div class="meta-info">
      작성일: 2026.01.28<br />
    </div>

    <p style="text-align:center;">
      <strong>배터리 설치 과정 사진</strong>
    </p>

    <div class="photo-gallery">
      <div class="photo-item">
        <img src="https://bf.sbgpt.ddns.net/download/0351e896-0f79-4290-9024-f1c08b35ae4c_minseo.png" alt="배터리 설치 준비 사진">
        <div class="photo-caption">배터리 설치 준비 사진</div>
      </div>

      <div class="photo-item">
        <img src="https://bf.sbgpt.ddns.net/download/de9f5b23-f711-4d2a-9472-d497ad1fb1f9_minseo.png" alt="배터리 철거 후 사진">
        <div class="photo-caption">배터리 철거 후 사진</div>
      </div>

      <div class="photo-item">
        <img src="https://bf.sbgpt.ddns.net/download/640bc2dc-93ee-4055-a3d9-4e7509abcfd2_minseo.png" alt="배터리 설치 후 사진">
        <div class="photo-caption">배터리 설치 후 사진</div>
      </div>

      <div class="photo-item">
        <img src="https://bf.sbgpt.ddns.net/download/c7b37407-26cd-4d1b-8bd9-9bb8628509a0_minseo.png" alt="배터리 전압 체크 사진">
        <div class="photo-caption">배터리 전압 체크 사진</div>
      </div>

      <div class="photo-item full-width">
        <img src="https://bf.sbgpt.ddns.net/download/63156cc0-b219-42c4-81c0-3ff46a63aa74_minseo1.jpg" alt="설치 위치 장소 사진">
        <div class="photo-caption">설치 위치 장소 사진</div>
      </div>
    </div>

    <!-- ✅ 블로그 글 작성 영역 (설치 장소 사진 아래 / 담당자 섹션 위) -->
    <section class="post-section">
      <h2 class="post-title">작업 후기 / 상세 내용</h2>

      <div class="post-body">
        <p>
          이번에 서울대학교 병원에서 UPS 시스템의 배터리 교체 작업을 완료했습니다.
          기존에 사용하던 배터리가 수명이 다되어 안정적인 전력 공급이 어려웠던 상황에서
          새로운 배터리로 교체하여 시스템의 신뢰성을 높였습니다.
        </p>

        <p>
          설치 과정은 전문적인 팀이 진행했으며, 모든 단계에서 안전 규정을 준수했습니다.
          특히 배터리 철거 시에는 전원 차단 및 방전 작업을 꼼꼼히 수행하여
          장비 손상이나 사고 예방에 최선을 다했습니다.
        </p>

        <div class="info-box">
          <p><strong>✅ 작업 요약</strong></p>
          <ul>
            <li>기존 배터리 철거 및 분리</li>
            <li>신규 배터리 설치 및 배선 정리</li>
            <li>전압/충전 상태 확인 및 최종 동작 테스트</li>
          </ul>
        </div>

        <p>
          설치 후에는 전압 및 충전 상태를 체크하고, UPS 시스템이 정상적으로 작동하는지
          여러 가지 테스트를 진행했습니다. 모든 테스트 결과 정상적으로 작동함을 확인하였습니다.
          이로 인해 병원 내 의료 장비의 지속적인 전력 공급이 가능해졌습니다.
        </p>
      </div>
    </section>

    <div class="contact-section">
      <p class="contact-title">담당자 / 연락처</p>

      <div class="contact-grid">
        <!-- ✅ 내 사진 (왼쪽 1) -->
        <div class="card-box">
          <div class="profile-photo">
            <img src="https://bf.sbgpt.ddns.net/download/c6d65572-a442-416e-a71a-5561a7e0e929_minseo.png" alt="프로필 사진" />
          </div>
        </div>

        <!-- ✅ 명함 (오른쪽 3) -->
        <div class="card-box">
          <div class="business-card-img"
               data-full="https://bf.sbgpt.ddns.net/download/6db2b14e-cac3-4e62-8282-baeacbd00b94_minseo1.jpg">
            <img src="https://bf.sbgpt.ddns.net/download/6db2b14e-cac3-4e62-8282-baeacbd00b94_minseo1.jpg" alt="명함 이미지" />
          </div>
        </div>
      </div>
    </div>

    <div class="footer">
      본 글은 2026년 1월 28일 진행된 서울대학교 병원 UPS 배터리 설치 작업을 기반으로 작성되었습니다.
    </div>
  </div>

  <!-- 라이트박스 -->
  <div class="lightbox" id="lightbox">
    <div class="lightbox-content">
      <button class="lightbox-close" id="close">×</button>
      <img id="lightboxImg" alt="명함 확대" />
    </div>
  </div>

  <script>
    const lb = document.getElementById("lightbox");
    const lbImg = document.getElementById("lightboxImg");
    const close = document.getElementById("close");

    document.querySelectorAll(".business-card-img").forEach((el) => {
      el.onclick = () => {
        lbImg.src = el.dataset.full;
        lb.classList.add("open");
        document.body.style.overflow = "hidden";
      };
    });

    close.onclick = () => {
      lb.classList.remove("open");
      document.body.style.overflow = "";
    };

    lb.onclick = (e) => {
      if (e.target === lb) close.onclick();
    };
  </script>
</body>
</html>
