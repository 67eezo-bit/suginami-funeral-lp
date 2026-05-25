/* =========================================================
   Suginami LP - Main JavaScript
   FAQアコーディオン + スクロールリビール
   ========================================================= */

(function () {
  'use strict';

  /* ---------- FAQ Accordion ---------- */
  function initFaq() {
    var items = document.querySelectorAll('.faq-item');
    items.forEach(function (item, idx) {
      var btn = item.querySelector('.faq-item__button');
      if (!btn) return;

      // 初期: 最初の項目だけ開く
      if (idx === 0) item.classList.add('is-open');

      btn.addEventListener('click', function () {
        var isOpen = item.classList.contains('is-open');
        // 他を閉じる（アコーディオン挙動）。複数同時オープンにしたい場合はこの forEach を削除。
        items.forEach(function (other) {
          if (other !== item) other.classList.remove('is-open');
        });
        if (isOpen) {
          item.classList.remove('is-open');
        } else {
          item.classList.add('is-open');
        }
        btn.setAttribute('aria-expanded', String(!isOpen));
      });
    });
  }

  /* ---------- Reveal on scroll (IntersectionObserver) ---------- */
  function initReveal() {
    if (!('IntersectionObserver' in window)) return;
    var targets = document.querySelectorAll('[data-reveal]');
    if (targets.length === 0) return;

    var observer = new IntersectionObserver(function (entries) {
      entries.forEach(function (entry) {
        if (entry.isIntersecting) {
          entry.target.classList.add('reveal');
          observer.unobserve(entry.target);
        }
      });
    }, { threshold: 0.12, rootMargin: '0px 0px -40px 0px' });

    targets.forEach(function (el) { observer.observe(el); });
  }

  /* ---------- Current year in footer ---------- */
  function initYear() {
    var yearEl = document.getElementById('current-year');
    if (yearEl) yearEl.textContent = new Date().getFullYear();
  }

  /* ---------- Init on DOM ready ---------- */
  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', function () {
      initFaq();
      initReveal();
      initYear();
    });
  } else {
    initFaq();
    initReveal();
    initYear();
  }
})();
