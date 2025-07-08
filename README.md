# Oohsha.github.io
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <title>안심귀가</title>
  <link
    rel="stylesheet"
    href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
  />
  <style>
    #map {
      width: 100%;
      height: 700px;
    }
    .feedback-btn {
      margin-top: 8px;
      padding: 6px 10px;
      background-color: #28a745;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      font-weight: bold;
    }
    .feedback-btn:disabled {
      background-color: #a0a0a0;
      cursor: default;
    }
  </style>
</head>
<body>
  <h2>안심 귀가 지도</h2>
  <div id="map"></div>

  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

  <script>
    const marker = [
      {
        lat: 37.5665,
        lng: 126.9780,
        description: "주택가 주변, 현재 밤 10시 기준 조명이 약함",
        feedbackCount: 0,
      },
      {
        lat: 37.5700,
        lng: 126.9820,
        description: "공원 근처, 인적이 드물고 CCTV 적음",
        feedbackCount: 0,
      },
      {
        lat: 37.5640,
        lng: 126.9750,
        description: "버스 정류장 인근, 유동인구 많음",
        feedbackCount: 0,
      },
      {
        lat: 37.5678,
        lng: 126.9812,
        description: "골목길, 최근 신고 사례 있음",
        feedbackCount: 0,
      },
      {
        lat: 37.5708,
        lng: 126.9842,
        description: "골목길, 인적이 드물지만 CCTV 많음 ",
        feedbackCount: 0,
      },
    ];

    const map = L.map("map").setView([37.5665, 126.9780], 14);

    L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
      maxZoom: 19,
      attribution:
        '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
    }).addTo(map);

    function onFeedbackClick(index, button, feedbackSpan) {
      marker[index].feedbackCount++;
      feedbackSpan.textContent = marker[index].feedbackCount;
      button.disabled = true;
      alert("피드백이 반영되었습니다.");
    }

    marker.forEach((loc, index) => {
      const marker = L.marker([loc.lat, loc.lng]).addTo(map);

      const popupContent = document.createElement("div");
      popupContent.style.fontFamily = "'Noto Sans KR', sans-serif";
      popupContent.style.fontSize = "14px";

      const descDiv = document.createElement("div");
      descDiv.innerHTML = `<strong>상황 안내:</strong><br>${loc.description}`;
      popupContent.appendChild(descDiv);

      const feedbackDiv = document.createElement("div");
      feedbackDiv.style.marginTop = "8px";
      feedbackDiv.innerHTML = `피드백: <span id="feedback-count-${index}">${loc.feedbackCount}</span>명 이 길 괜찮다고 평가함`;
      popupContent.appendChild(feedbackDiv);

      const button = document.createElement("button");
      button.className = "feedback-btn";
      button.textContent = "이 길 괜찮?";
      popupContent.appendChild(button);

      button.addEventListener("click", () => {
        onFeedbackClick(index, button, feedbackDiv.querySelector("span"));
      });

      marker.bindPopup(popupContent);
    });
  </script>
</body>
</html>
