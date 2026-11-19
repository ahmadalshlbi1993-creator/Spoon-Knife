ه<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>الصفقة السورية</title>
  <link rel="manifest" href="manifest.json" />
  <meta name="theme-color" content="#0f5132" />
  <style>
    :root {
      --bg: #f0f2f5;
      --card: #ffffff;
      --primary: #0f5132;
      --primary-hover: #0b3d26;
      --text: #1c1e21;
      --muted: #65676b;
      --border: #dcdfe3;
      --accent: #e9f5ee;
      --warn-bg: #fff8e1;
      --warn-text: #7a5200;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      font-family: system-ui, -apple-system, "Segoe UI", Tahoma, Arial, sans-serif;
      background: var(--bg);
      color: var(--text);
      line-height: 1.5;
    }

    header {
      position: sticky;
      top: 0;
      z-index: 20;
      background: var(--card);
      border-bottom: 1px solid var(--border);
      padding: 12px 16px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 12px;
    }

    .brand {
      font-size: 1.25rem;
      font-weight: 800;
      color: var(--primary);
    }

    .header-actions {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
    }

    button, select, input, textarea {
      font: inherit;
      border-radius: 8px;
      border: 1px solid var(--border);
    }

    button {
      cursor: pointer;
      padding: 8px 14px;
      background: var(--primary);
      color: #fff;
      border: none;
      font-weight: 600;
    }

    button:hover { background: var(--primary-hover); }
    button.secondary {
      background: #eef0f3;
      color: var(--text);
    }
    button.secondary:hover { background: #e2e5e9; }
    button.whatsapp {
      background: #16a34a;
      color: #fff;
    }

    .container {
      max-width: 760px;
      margin: 16px auto;
      padding: 0 12px 48px;
    }

    .panel {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 16px;
      margin-bottom: 16px;
    }

    .panel h2, .panel h3 {
      margin: 0 0 12px;
      font-size: 1.05rem;
    }

    .grid-2 {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(210px, 1fr));
      gap: 10px;
    }

    input, select, textarea {
      width: 100%;
      padding: 10px 12px;
      background: #fff;
      color: var(--text);
    }

    textarea { min-height: 90px; resize: vertical; }

    .filters {
      display: grid;
      grid-template-columns: 1fr 1fr auto;
      gap: 10px;
      align-items: center;
    }

    @media (max-width: 600px) {
      .filters { grid-template-columns: 1fr; }
    }

    .post {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 16px;
      margin-bottom: 14px;
    }

    .post-meta {
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 8px;
      margin-bottom: 8px;
      font-size: 0.9rem;
      color: var(--muted);
    }

    .badges {
      display: flex;
      gap: 6px;
      flex-wrap: wrap;
    }

    .badge {
      background: var(--accent);
      color: var(--primary);
      padding: 3px 9px;
      border-radius: 999px;
      font-size: 0.8rem;
      font-weight: 700;
    }

    .price {
      font-weight: 800;
      color: var(--primary);
      font-size: 1.05rem;
      margin: 6px 0;
    }

    .media-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
      gap: 8px;
      margin: 10px 0;
    }

    .media-grid img, .media-grid video {
      width: 100%;
      max-height: 280px;
      object-fit: cover;
      border-radius: 8px;
      background: #000;
    }

    .post-actions {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
      margin-top: 10px;
      padding-top: 10px;
      border-top: 1px solid var(--border);
    }

    .comments-box {
      margin-top: 12px;
      padding-top: 10px;
      border-top: 1px dashed var(--border);
    }

    .comment-item {
      background: #f7f8fa;
      padding: 8px 10px;
      border-radius: 8px;
      margin-bottom: 6px;
      font-size: 0.9rem;
    }

    .notice {
      background: var(--warn-bg);
      color: var(--warn-text);
      padding: 10px 12px;
      border-radius: 8px;
      font-size: 0.9rem;
      margin-bottom: 12px;
    }

    .hidden { display: none; }
  </style>
</head>
<body>
  <header>
    <div class="brand">الصفقة السورية</div>
    <div class="header-actions">
      <button type="button" id="toggleCreateBtn">إضافة إعلان</button>
      <button type="button" class="secondary" id="toggleAdminBtn">إدارة الإعلانات</button>
    </div>
  </header>

  <main class="container">
    <!-- شريط التصفية حسب المحافظة والتصنيف -->
    <section class="panel">
      <div class="filters">
        <select id="filterGov" aria-label="المحافظة">
          <option value="">كل المحافظات</option>
        </select>
        <select id="filterCat" aria-label="التصنيف">
          <option value="">كل التصنيفات</option>
        </select>
        <button type="button" class="secondary" id="resetFiltersBtn">إعادة ضبط</button>
      </div>
    </section>

    <!-- نموذج إضافة إعلان جديد دون اشتراط تأكيد بريد إلكتروني -->
    <section class="panel hidden" id="createPanel">
      <h2>نشر إعلان جديد</h2>
      <div id="monetizationBanner" class="notice"></div>
      <form id="postForm">
        <div class="grid-2">
          <input type="text" id="authorName" placeholder="الاسم أو اسم المعرض / المكتب" required />
          <input type="text" id="whatsappPhone" placeholder="رقم واتساب (مثال: 9639xxxxxxxx)" required />
          <select id="postGov" required></select>
          <select id="postCat" required></select>
          <input type="text" id="postPrice" placeholder="السعر (مثال: 25,000$ أو 120$ شهرياً)" required />
          <input type="text" id="shamCashRef" placeholder="رقم عملية شام كاش (مطلوب عند تفعيل الدفع)" />
        </div>
        <div style="margin-top: 10px;">
          <textarea id="postDesc" placeholder="اكتب تفاصيل العقار أو السيارة أو المعرض أو الإعلان التجاري..." required></textarea>
        </div>
        <div style="margin-top: 10px;">
          <label style="display:block; margin-bottom:6px; font-size:0.9rem; color:var(--muted);">
            إرفاق صور أو مقاطع فيديو مباشرة من الجهاز:
          </label>
          <input type="file" id="postMedia" accept="image/*,video/*" multiple />
        </div>
        <div style="margin-top: 12px; display:flex; gap:8px;">
          <button type="submit">إرسال للمراجعة</button>
          <button type="button" class="secondary" id="cancelPostBtn">إلغاء</button>
        </div>
      </form>
    </section>

    <!-- لوحة الإدارة لاعتماد الإعلانات وإعدادات النشر وشام كاش -->
    <section class="panel hidden" id="adminPanel">
      <h2>لوحة إدارة الصفقة السورية</h2>
      <div id="adminAuthBox">
        <p style="margin-top:0; color:var(--muted); font-size:0.92rem;">
          أدخل رمز الإدارة للتحكم بحالة النشر (مجاني أو مدفوع بالدولار عبر شام كاش) واعتماد الإعلانات المعلقة.
        </p>
        <div class="grid-2">
          <input type="password" id="adminPassInput" placeholder="رمز الإدارة (الافتراضي: sydeal2026)" />
          <button type="button" id="adminLoginBtn">دخول الإدارة</button>
        </div>
      </div>

      <div id="adminControls" class="hidden">
        <div class="grid-2" style="margin-bottom: 14px;">
          <div>
            <label style="font-size:0.85rem; color:var(--muted);">وضع النشر الحالي</label>
            <select id="postingModeSelect">
              <option value="free">نشر مجاني مؤقت</option>
              <option value="paid">نشر مدفوع بالدولار (شام كاش)</option>
            </select>
          </div>
          <div>
            <label style="font-size:0.85rem; color:var(--muted);">رسوم الإعلان (بالدولار USD)</label>
            <input type="number" id="paidFeeUsd" min="1" step="0.5" value="5" />
          </div>
          <div>
            <label style="font-size:0.85rem; color:var(--muted);">حساب / معرف شام كاش</label>
            <input type="text" id="shamCashAccount" value="SHAM-CASH-ALSAFQA" />
          </div>
          <div style="display:flex; align-items:flex-end;">
            <button type="button" id="saveSettingsBtn" style="width:100%;">حفظ الإعدادات</button>
          </div>
        </div>

        <h3>إعلانات بانتظار الاعتماد</h3>
        <div id="pendingList"></div>
      </div>
    </section>

    <!-- موجز الإعلانات المعتمدة -->
    <section id="feedList"></section>
  </main>

  <script>
    const GOVERNORATES = [
      "دمشق", "ريف دمشق", "حلب", "حمص", "حماة", "اللاذقية",
      "طرطوس", "إدلب", "دير الزور", "الرقة", "الحسكة", "درعا", "السويداء", "القنيطرة"
    ];

    const CATEGORIES = [
      "عقارات", "سيارات", "معارض", "إعلانات تجارية"
    ];

    const STORAGE_KEY = "alsafqa_syria_state_v1";
    const ADMIN_CODE = "sydeal2026";

    const defaultState = {
      settings: {
        mode: "free",
        feeUsd: 5,
        shamCashAccount: "SHAM-CASH-ALSAFQA"
      },
      posts: [
        {
          id: "p-101",
          author: "مكتب الياسمين للعقارات",
          whatsapp: "963944112233",
          gov: "دمشق",
          cat: "عقارات",
          price: "48,000$",
          desc: "شقة طابق ثالث بمساحة 125 م²، إكساء جيد، قريبة من المواصلات والخدمات.",
          media: [],
          shamCashRef: "",
          status: "approved",
          createdAt: "منذ ساعة",
          comments: ["هل السعر قابل للتفاوض؟"]
        },
        {
          id: "p-102",
          author: "معرض الشهباء للسيارات",
          whatsapp: "963955667788",
          gov: "حلب",
          cat: "سيارات",
          price: "9,500$",
          desc: "سيارة بحالة ممتازة، فحص كامل، جاهزة للفراغ الفوري.",
          media: [],
          shamCashRef: "",
          status: "approved",
          createdAt: "منذ ساعتين",
          comments: []
        }
      ]
    };

    let state = loadState();
    let isAdminLoggedIn = false;

    function loadState() {
      try {
        const raw = localStorage.getItem(STORAGE_KEY);
        if (!raw) return structuredClone(defaultState);
        const parsed = JSON.parse(raw);
        return {
          settings: { ...defaultState.settings, ...(parsed.settings || {}) },
          posts: Array.isArray(parsed.posts) ? parsed.posts : defaultState.posts
        };
      } catch {
        return structuredClone(defaultState);
      }
    }

    function saveState() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
    }

    function populateSelects() {
      const filterGov = document.getElementById("filterGov");
      const filterCat = document.getElementById("filterCat");
      const postGov = document.getElementById("postGov");
      const postCat = document.getElementById("postCat");

      GOVERNORATES.forEach((g) => {
        filterGov.add(new Option(g, g));
        postGov.add(new Option(g, g));
      });

      CATEGORIES.forEach((c) => {
        filterCat.add(new Option(c, c));
        postCat.add(new Option(c, c));
      });
    }

    function updateMonetizationUI() {
      const banner = document.getElementById("monetizationBanner");
      const shamInput = document.getElementById("shamCashRef");

      if (state.settings.mode === "paid") {
        banner.innerHTML = `النشر حالياً مدفوع بقيمة <strong>${state.settings.feeUsd}$</strong> عبر <strong>شام كاش</strong> إلى الحساب: <strong>${escapeHtml(state.settings.shamCashAccount)}</strong>. أدخل رقم العملية ليتم اعتماد إعلانك.`;
        shamInput.required = true;
      } else {
        banner.innerHTML = `النشر حالياً <strong>مجاني</strong> لفترة محدودة. سيظهر إعلانك في الموجز فور اعتماده من الإدارة.`;
        shamInput.required = false;
      }
    }

    function escapeHtml(str) {
      return String(str || "")
        .replaceAll("&", "&amp;")
        .replaceAll("<", "&lt;")
        .replaceAll(">", "&gt;")
        .replaceAll('"', "&quot;");
    }

    function readFilesAsDataUrls(fileList) {
      const files = Array.from(fileList || []);
      return Promise.all(
        files.map((file) => new Promise((resolve) => {
          const reader = new FileReader();
          reader.onload = () => resolve({
            type: file.type.startsWith("video/") ? "video" : "image",
            src: reader.result
          });
          reader.readAsDataURL(file);
        }))
      );
    }

    function renderFeed() {
      const feedList = document.getElementById("feedList");
      const selectedGov = document.getElementById("filterGov").value;
      const selectedCat = document.getElementById("filterCat").value;

      const approvedPosts = state.posts.filter((p) => {
        if (p.status !== "approved") return false;
        if (selectedGov && p.gov !== selectedGov) return false;
        if (selectedCat && p.cat !== selectedCat) return false;
        return true;
      });

      if (!approvedPosts.length) {
        feedList.innerHTML = `<div class="panel" style="text-align:center; color:var(--muted);">لا توجد إعلانات مطابقة للتصفية الحالية.</div>`;
        return;
      }

      feedList.innerHTML = approvedPosts.map((post) => {
        const mediaHtml = (post.media || []).map((m) =>
          m.type === "video"
            ? `<video src="${m.src}" controls playsinline></video>`
            : `<img src="${m.src}" alt="${escapeHtml(post.cat)}" />`
        ).join("");

        const commentsHtml = (post.comments || []).map((c) =>
          `<div class="comment-item">${escapeHtml(c)}</div>`
        ).join("");

        const cleanPhone = (post.whatsapp || "").replace(/\D/g, "");
        const waText = encodeURIComponent(`مرحباً، بخصوص إعلان (${post.cat} - ${post.gov}): ${post.price}`);
        const waUrl = `https://wa.me/${cleanPhone}?text=${waText}`;

        return `
          <article class="post">
            <div class="post-meta">
              <div>
                <strong>${escapeHtml(post.author)}</strong>
                <span> · ${escapeHtml(post.createdAt)}</span>
              </div>
              <div class="badges">
                <span class="badge">${escapeHtml(post.gov)}</span>
                <span class="badge">${escapeHtml(post.cat)}</span>
              </div>
            </div>
            <div class="price">${escapeHtml(post.price)}</div>
            <p style="margin: 6px 0 10px;">${escapeHtml(post.desc)}</p>
            ${mediaHtml ? `<div class="media-grid">${mediaHtml}</div>` : ""}
            <div class="post-actions">
              <a href="${waUrl}" target="_blank" rel="noopener noreferrer">
                <button type="button" class="whatsapp">تواصل واتساب</button>
              </a>
              <button type="button" class="secondary" onclick="sharePost('${post.id}')">مشاركة</button>
            </div>
            <div class="comments-box">
              <div id="comments-${post.id}">${commentsHtml}</div>
              <form onsubmit="addComment(event, '${post.id}')" style="display:flex; gap:6px; margin-top:8px;">
                <input type="text" placeholder="أضف تعليقاً أو استفساراً..." required />
                <button type="submit" class="secondary">تعليق</button>
              </form>
            </div>
          </article>
        `;
      }).join("");
    }

    function renderPending() {
      const pendingList = document.getElementById("pendingList");
      const pending = state.posts.filter((p) => p.status === "pending");

      if (!pending.length) {
        pendingList.innerHTML = `<p style="color:var(--muted); font-size:0.9rem;">لا توجد إعلانات معلقة حالياً.</p>`;
        return;
      }

      pendingList.innerHTML = pending.map((p) => `
        <div class="post" style="background:#fafafa;">
          <div class="post-meta">
            <strong>${escapeHtml(p.author)} (${escapeHtml(p.gov)} - ${escapeHtml(p.cat)})</strong>
            <span class="price">${escapeHtml(p.price)}</span>
          </div>
          <p style="margin:6px 0;">${escapeHtml(p.desc)}</p>
          ${p.shamCashRef ? `<div style="font-size:0.85rem; color:var(--primary); margin-bottom:8px;">مرجع شام كاش: <strong>${escapeHtml(p.shamCashRef)}</strong></div>` : ""}
          <div style="display:flex; gap:8px;">
            <button type="button" onclick="moderatePost('${p.id}', 'approved')">اعتماد ونشر</button>
            <button type="button" class="secondary" onclick="moderatePost('${p.id}', 'rejected')">رفض</button>
          </div>
        </div>
      `).join("");
    }

    window.addComment = function (event, postId) {
      event.preventDefault();
      const input = event.target.querySelector("input");
      const text = input.value.trim();
      if (!text) return;
      const post = state.posts.find((p) => p.id === postId);
      if (!post) return;
      post.comments = post.comments || [];
      post.comments.push(text);
      saveState();
      input.value = "";
      renderFeed();
    };

    window.sharePost = function (postId) {
      const post = state.posts.find((p) => p.id === postId);
      if (!post) return;
      const text = `${post.cat} في ${post.gov} - ${post.price}\n${post.desc}`;
      if (navigator.share) {
        navigator.share({ title: "الصفقة السورية", text }).catch(() => {});
      } else if (navigator.clipboard) {
        navigator.clipboard.writeText(text);
        alert("تم نسخ تفاصيل الإعلان للمشاركة.");
      }
    };

    window.moderatePost = function (postId, status) {
      const post = state.posts.find((p) => p.id === postId);
      if (!post) return;
      post.status = status;
      saveState();
      renderPending();
      renderFeed();
    };

    document.getElementById("toggleCreateBtn").addEventListener("click", () => {
      document.getElementById("createPanel").classList.toggle("hidden");
      updateMonetizationUI();
    });

    document.getElementById("cancelPostBtn").addEventListener("click", () => {
      document.getElementById("createPanel").classList.add("hidden");
    });

    document.getElementById("toggleAdminBtn").addEventListener("click", () => {
      document.getElementById("adminPanel").classList.toggle("hidden");
      if (isAdminLoggedIn) renderPending();
    });

    document.getElementById("adminLoginBtn").addEventListener("click", () => {
      const code = document.getElementById("adminPassInput").value.trim();
      if (code === ADMIN_CODE) {
        isAdminLoggedIn = true;
        document.getElementById("adminAuthBox").classList.add("hidden");
        document.getElementById("adminControls").classList.remove("hidden");
        document.getElementById("postingModeSelect").value = state.settings.mode;
        document.getElementById("paidFeeUsd").value = state.settings.feeUsd;
        document.getElementById("shamCashAccount").value = state.settings.shamCashAccount;
        renderPending();
      } else {
        alert("رمز الإدارة غير صحيح.");
      }
    });

    document.getElementById("saveSettingsBtn").addEventListener("click", () => {
      state.settings.mode = document.getElementById("postingModeSelect").value;
      state.settings.feeUsd = Number(document.getElementById("paidFeeUsd").value) || 5;
      state.settings.shamCashAccount = document.getElementById("shamCashAccount").value.trim() || "SHAM-CASH-ALSAFQA";
      saveState();
      updateMonetizationUI();
      alert("تم حفظ إعدادات النشر وشام كاش.");
    });

    document.getElementById("postForm").addEventListener("submit", async (e) => {
      e.preventDefault();
      const media = await readFilesAsDataUrls(document.getElementById("postMedia").files);
      const newPost = {
        id: "p-" + Date.now(),
        author: document.getElementById("authorName").value.trim(),
        whatsapp: document.getElementById("whatsappPhone").value.trim(),
        gov: document.getElementById("postGov").value,
        cat: document.getElementById("postCat").value,
        price: document.getElementById("postPrice").value.trim(),
        desc: document.getElementById("postDesc").value.trim(),
        shamCashRef: document.getElementById("shamCashRef").value.trim(),
        media,
        status: "pending",
        createdAt: "الآن",
        comments: []
      };
      state.posts.unshift(newPost);
      saveState();
      e.target.reset();
      document.getElementById("createPanel").classList.add("hidden");
      if (isAdminLoggedIn) renderPending();
      alert("تم إرسال إعلانك بنجاح وسيظهر بعد اعتماد الإدارة.");
    });

    document.getElementById("filterGov").addEventListener("change", renderFeed);
    document.getElementById("filterCat").addEventListener("change", renderFeed);
    document.getElementById("resetFiltersBtn").addEventListener("click", () => {
      document.getElementById("filterGov").value = "";
      document.getElementById("filterCat").value = "";
      renderFeed();
    });

    if ("serviceWorker" in navigator) {
      navigator.serviceWorker.register("sw.js").catch(() => {});
    }

    populateSelects();
    updateMonetizationUI();
    renderFeed();
  </script>
</body>
</html>
