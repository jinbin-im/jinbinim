---
layout: about
title: Conference
permalink: /conference/
---

<section id="conferences" class="conferences-section">
  <h2>Conference</h2>
  
  <div class="publications">
    {% bibliography --file conferences --template conferences %}
  </div>
</section>

<script>
// Conference 아코디언
document.addEventListener('DOMContentLoaded', function() {
  setTimeout(function() {
    const publications = document.querySelector('#conferences .publications');
    if (!publications) return;
    
    const rows = Array.from(publications.querySelectorAll('.conference-row'));
    if (rows.length === 0) return;
    
    // 연도별, 컨퍼런스별 그룹핑
    const yearGroups = {};
    
    rows.forEach(function(row) {
      const year = row.getAttribute('data-year') || 'Unknown';
      const conference = row.getAttribute('data-conference') || 'Other';
      const booktitle = row.getAttribute('data-booktitle') || '';
      const address = row.getAttribute('data-address') || '';
      const monthValue = row.getAttribute('data-month') || '';
      
      if (!yearGroups[year]) {
        yearGroups[year] = {};
      }
      
      if (!yearGroups[year][conference]) {
        yearGroups[year][conference] = {
          rows: [],
          booktitle: booktitle,
          address: address,
          month: monthValue
        };
      }
      
      yearGroups[year][conference].rows.push(row);
    });
    
    // 기존 내용 제거
    publications.innerHTML = '';
    
    // 연도별로 정렬 (최신순)
    const sortedYears = Object.keys(yearGroups).sort().reverse();
    
    sortedYears.forEach(function(year) {
      // 연도 헤더
      const yearHeader = document.createElement('h2');
      yearHeader.className = 'bibliography';
      yearHeader.textContent = year;
      publications.appendChild(yearHeader);
      
      // 컨퍼런스들
      const conferences = Object.keys(yearGroups[year]).sort();
      
      conferences.forEach(function(conference) {
        const group = yearGroups[year][conference];
        
        // 컨퍼런스 헤더 박스
        const header = document.createElement('div');
        header.className = 'conference-header collapsed';
        header.setAttribute('data-conference', conference);
        
        const title = document.createElement('h3');
        
        const confName = document.createElement('span');
        confName.className = 'conference-name';
        confName.textContent = conference;
        title.appendChild(confName);
        
        const monthSpan = document.createElement('span');
        monthSpan.className = 'conference-month';
        monthSpan.textContent = group.month || '';
        title.appendChild(monthSpan);
        
        const addressSpan = document.createElement('span');
        addressSpan.className = 'conference-address';
        addressSpan.textContent = group.address || '';
        title.appendChild(addressSpan);
        
        header.appendChild(title);
        
        if (group.booktitle) {
          const fullTitle = document.createElement('div');
          fullTitle.className = 'conference-full-title';
          fullTitle.textContent = group.booktitle;
          header.appendChild(fullTitle);
        }
        
        publications.appendChild(header);
        
        // 논문 컨테이너
        const papersContainer = document.createElement('div');
        papersContainer.className = 'conference-papers hidden';
        
        group.rows.forEach(function(row) {
          papersContainer.appendChild(row);
        });
        
        publications.appendChild(papersContainer);
        
        // 클릭 토글
        header.addEventListener('click', function() {
          header.classList.toggle('collapsed');
          papersContainer.classList.toggle('hidden');
        });
      });
    });
  }, 200);
});
</script>

<style>
/* 컨퍼런스별 고유 색상 */
.conference-header[data-conference*="ICCEPM 2024"] {
  border-left-color: #858f99;
}
.conference-header[data-conference*="ICCEPM 2025"] {
  border-left-color: #164b9d;
}
.conference-header[data-conference*="ISARC 2025"] {
  border-left-color: #8d2214;
}
.conference-header[data-conference*="ISARC 2026"] {
  border-left-color: #7b27d8;
}
.conference-header[data-conference*="i3CE 2026"],
.conference-header[data-conference*="I3CE 2026"] {
  border-left-color: #152d97;
}
.conference-header[data-conference*="IAQVEC 2026"] {
  border-left-color: #b58c65;
}
</style>

<!-- Copyright Footer -->
<div class="site-footer">
      <p>&copy; 2026 Jinbin Im. All rights reserved.</p>
</div>
