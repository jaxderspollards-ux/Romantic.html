<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <title>💊 和好胶囊 · 时光胶卷</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body {
      font-family: 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', sans-serif;
      background: linear-gradient(135deg, #fce4ec 0%, #ffe0f0 50%, #f8d7e6 100%);
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 20px;
      position: relative;
      overflow-x: hidden;
    }
    .container {
      max-width: 420px;
      width: 100%;
      background: rgba(255, 255, 255, 0.85);
      backdrop-filter: blur(20px);
      border-radius: 40px;
      box-shadow: 0 25px 50px rgba(255, 105, 135, 0.2);
      padding: 30px 25px;
      text-align: center;
      position: relative;
      z-index: 1;
      transition: all 0.4s ease;
    }
    h1 { font-size: 26px; color: #d6336c; margin-bottom: 5px; font-weight: 700; letter-spacing: 2px; }
    .sub-title { font-size: 14px; color: #e6739f; margin-bottom: 25px; font-style: italic; }

    /* 上传区 */
    .upload-area {
      border: 2px dashed #faa2c1;
      border-radius: 24px;
      padding: 30px 20px;
      background: rgba(255, 218, 224, 0.3);
      cursor: pointer;
      transition: all 0.3s;
      margin-bottom: 15px;
    }
    .upload-icon { font-size: 48px; margin-bottom: 10px; }
    .upload-text { color: #d6336c; font-size: 15px; font-weight: 500; }

    /* 胶卷播放区 */
    .film-viewer {
      width: 100%;
      height: 300px;
      border-radius: 24px;
      background: #1a1a1a;
      overflow: hidden;
      position: relative;
      margin: 20px 0;
      display: none;
      box-shadow: inset 0 0 30px rgba(0,0,0,0.8);
    }
    .film-viewer::before,
    .film-viewer::after {
      content: '';
      position: absolute;
      left: 0;
      width: 100%;
      height: 18px;
      background: repeating-linear-gradient(90deg, #222 0px, #222 8px, transparent 8px, transparent 16px);
      z-index: 5;
      pointer-events: none;
    }
    .film-viewer::before { top: 0; }
    .film-viewer::after { bottom: 0; }
    .film-strip {
      display: flex;
      height: 100%;
      align-items: center;
      position: absolute;
      top: 0;
      left: 0;
      will-change: transform;
    }
    .film-strip img {
      height: 85%;
      max-height: 250px;
      width: auto;
      margin: 0 8px;
      border-radius: 12px;
      box-shadow: 0 8px 20px rgba(0,0,0,0.5);
      border: 2px solid #ffd1dc;
      background: #fff;
      object-fit: cover;
      min-width: 160px;
    }

    /* 和好券 */
    .coupon-section {
      display: none;
      animation: fadeInUp 0.6s ease;
    }
    @keyframes fadeInUp {
      from { opacity: 0; transform: translateY(30px); }
      to { opacity: 1; transform: translateY(0); }
    }
    .coupon-card {
      background: linear-gradient(145deg, #fff5f8, #ffe0ec);
      border-radius: 30px;
      padding: 30px 20px;
      margin: 20px 0;
      border: 2px solid #ffb3c6;
      box-shadow: 0 15px 30px rgba(255, 105, 135, 0.2);
    }
    .coupon-title { font-size: 24px; color: #d6336c; font-weight: 700; margin-bottom: 10px; }
    .coupon-desc { font-size: 15px; color: #555; margin-bottom: 25px; }
    .forgive-btn {
      background: #ff4d6d;
      font-size: 18px;
      padding: 15px 35px;
      border: none;
      color: white;
      border-radius: 50px;
      font-weight: 600;
      letter-spacing: 1px;
      cursor: pointer;
      box-shadow: 0 8px 20px rgba(255, 77, 109, 0.4);
      animation: pulse 1.8s infinite;
      width: 80%;
      margin: 0 auto;
    }
    @keyframes pulse {
      0% { box-shadow: 0 0 0 0 rgba(255, 77, 109, 0.5); }
      70% { box-shadow: 0 0 0 15px rgba(255, 77, 109, 0); }
      100% { box-shadow: 0 0 0 0 rgba(255, 77, 109, 0); }
    }

    /* 已使用 */
    .used-section { display: none; }
    .used-icon { font-size: 70px; margin-bottom: 15px; }
    .note { font-size: 12px; color: #999; margin-top: 20px; font-style: italic; }
    .hidden { display: none !important; }
    .btn {
      background: linear-gradient(135deg, #ff6b9d, #c44569);
      border: none;
      color: white;
      padding: 12px 25px;
      border-radius: 50px;
      font-size: 16px;
      font-weight: 600;
      cursor: pointer;
      box-shadow: 0 6px 15px rgba(255, 107, 157, 0.3);
      width: 80%;
      margin: 15px auto 0;
    }
  </style>
</head>
<body>
  <div class="container" id="mainContainer">
    <!-- 上传区 -->
    <div id="uploadSection">
      <h1>📸 你们的时光胶囊</h1>
      <p class="sub-title">选择一些属于你们的回忆照片吧</p>
      <div class="upload-area" id="uploadTrigger">
        <div class="upload-icon">🖼️</div>
        <div class="upload-text">点击这里选择照片 (2-8张)</div>
      </div>
      <input type="file" id="fileInput" multiple accept="image/*" style="display: none;">
      <p class="note">💡 选完后会自动播放胶卷哦</p>
    </div>

    <!-- 胶卷播放区 -->
    <div id="filmSection" style="display: none;">
      <h1>🎞️ 回忆正在播放...</h1>
      <div class="film-viewer" id="filmViewer">
        <div class="film-strip" id="filmStrip"></div>
      </div>
      <p class="sub-title" id="filmHint">让胶卷慢慢转动，重温那些美好</p>
    </div>

    <!-- 和好券 -->
    <div id="couponSection" class="coupon-section" style="display: none;">
      <h1>💊 和好胶囊</h1>
      <div class="coupon-card">
        <div class="coupon-title">专属和好券</div>
        <div class="coupon-desc">看完我们的回忆，<br>你愿意给彼此一个重新开始的机会吗？</div>
        <button class="forgive-btn" id="forgiveBtn">💖 原谅TA，点击和好</button>
      </div>
      <p class="note">⚠️ 这张和好券只能使用一次哦</p>
    </div>

    <!-- 已使用 -->
    <div id="usedSection" class="used-section" style="display: none;">
      <div class="used-icon">🌸</div>
      <h1>和好成功！</h1>
      <p style="color:#666; margin: 15px 0;">这份回忆和好券已经被珍藏<br>你们已经和好如初啦 💕</p>
      <p class="note">往后的日子，也要彼此珍惜</p>
    </div>
  </div>

  <script>
    (function() {
      const urlParams = new URLSearchParams(window.location.search);
      const token = urlParams.get('token') || 'default_capsule';
      const STORAGE_KEY = `capsule_used_${token}`;

      const uploadSection = document.getElementById('uploadSection');
      const filmSection = document.getElementById('filmSection');
      const couponSection = document.getElementById('couponSection');
      const usedSection = document.getElementById('usedSection');
      const fileInput = document.getElementById('fileInput');
      const uploadTrigger = document.getElementById('uploadTrigger');
      const filmViewer = document.getElementById('filmViewer');
      const filmStrip = document.getElementById('filmStrip');
      const filmHint = document.getElementById('filmHint');
      const forgiveBtn = document.getElementById('forgiveBtn');

      let animationTimer = null;

      // 检查是否已使用
      if (localStorage.getItem(STORAGE_KEY) === 'used') {
        uploadSection.style.display = 'none';
        filmSection.style.display = 'none';
        couponSection.style.display = 'none';
        usedSection.style.display = 'block';
        return;
      }

      // 上传
      uploadTrigger.addEventListener('click', () => fileInput.click());

      fileInput.addEventListener('change', (e) => {
        const files = Array.from(e.target.files);
        if (files.length === 0) return;

        // 限制最多8张
        const selectedFiles = files.slice(0, 8);

        // 立即切换到胶卷播放区并开始动画
        uploadSection.style.display = 'none';
        filmSection.style.display = 'block';
        loadAndPlay(selectedFiles);
      });

      function loadAndPlay(files) {
        filmStrip.innerHTML = '';
        const promises = files.map(file => {
          return new Promise((resolve) => {
            const reader = new FileReader();
            reader.onload = (e) => resolve(e.target.result);
            reader.readAsDataURL(file);
          });
        });

        Promise.all(promises).then(images => {
          images.forEach(src => {
            const img = document.createElement('img');
            img.src = src;
            filmStrip.appendChild(img);
          });

          // 等待图片加载
          waitForImages(filmStrip).then(() => {
            startFilmAnimation();
          });
        });
      }

      function waitForImages(container) {
        const imgs = container.querySelectorAll('img');
        const promises = Array.from(imgs).map(img => {
          return new Promise(resolve => {
            if (img.complete) resolve();
            else img.addEventListener('load', resolve);
          });
        });
        return Promise.all(promises);
      }

      function startFilmAnimation() {
        const stripWidth = filmStrip.scrollWidth;
        const viewerWidth = filmViewer.clientWidth;
        const totalDistance = stripWidth + viewerWidth;
        let duration = files.length * 2.5; // 每张2.5秒
        if (duration < 8) duration = 8;
        if (duration > 25) duration = 25;

        // 初始位置：胶卷最左端在右侧边缘外
        filmStrip.style.transition = 'none';
        filmStrip.style.transform = `translateX(${viewerWidth}px)`;
        filmStrip.offsetHeight; // 强制重排

        // 动画：向左移动
        filmHint.textContent = '回忆慢慢滑过，耐心看完哦...';
        filmStrip.style.transition = `transform ${duration}s linear`;
        filmStrip.style.transform = `translateX(-${stripWidth}px)`;

        animationTimer = setTimeout(() => {
          filmSection.style.display = 'none';
          couponSection.style.display = 'block';
        }, duration * 1000 + 500);
      }

      // 使用和好券
      forgiveBtn.addEventListener('click', () => {
        localStorage.setItem(STORAGE_KEY, 'used');
        couponSection.style.display = 'none';
        usedSection.style.display = 'block';
        if (animationTimer) clearTimeout(animationTimer);
      });

    })();
  </script>
</body>
</html>
