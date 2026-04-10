---
title: Hall of Fame
layout: default
permalink: /hall-of-fame/
icon: fas fa-trophy
order: 3
---
<div class="hof-container">
  <div class="hof-header">
    <h1>Hall of Fame</h1>
    <p>Celebrating our outstanding members and their achievements</p>
  </div>

  <div class="filter-bar" id="filter-bar">
    <button class="filter-btn active" data-year="all">All Years</button>
  </div>
  
  <div class="hof-grid" id="hof-grid"></div>
  <div class="loading" id="loading" style="display: none;">Loading...</div>
</div>

<script>
  const hofPosts = [
    {% for post in site.posts %}
      {% if post.categories contains "hall-of-fame" or post.path contains "hof" %}
        {% assign filename = post.path | split: '/' | last %}
        {% assign year = post.date | date: "%Y" %}
        {% assign image_name = post.path | replace: '.md', '.jpg' | replace: '_posts/', '' %}
        {
          url: "{{ post.url }}",
          image: "/assets/img/photos/{{ image_name }}",
          year: "{{ year }}",
          title: "{{ post.title }}",
          date: "{{ post.date | date: '%B %d, %Y' }}",
          external_link: "{{ post.external_link }}"
        },
      {% endif %}
    {% endfor %}
  ];

  console.log('Hall of Fame posts found:', hofPosts.length);

  const grid = document.getElementById('hof-grid');
  const filterBar = document.getElementById('filter-bar');
  
  if (hofPosts.length === 0) {
    grid.innerHTML = '<div class="no-posts">No Hall of Fame posts yet. Create a post with "hof" in the filename!</div>';
  } else {
    const years = [...new Set(hofPosts.map(p => p.year))].sort().reverse();
    
    years.forEach(year => {
      const btn = document.createElement('button');
      btn.className = 'filter-btn';
      btn.setAttribute('data-year', year);
      btn.textContent = year;
      filterBar.appendChild(btn);
    });
    
    hofPosts.forEach(post => {
      const item = document.createElement('a');
      item.className = 'hof-item';
      item.setAttribute('data-year', post.year);
      item.href = post.url;
      
      const img = document.createElement('img');
      img.src = post.image;
      img.alt = post.title;
      img.className = 'hof-image';
      img.loading = 'lazy';
      img.onerror = function() {
        this.src = 'https://via.placeholder.com/400x400?text=No+Image';
      };
      
      const overlay = document.createElement('div');
      overlay.className = 'hof-overlay';
      overlay.innerHTML = `
        <div class="hof-title">${post.title}</div>
        <div class="hof-date">${post.date}</div>
        ${post.external_link ? '<div class="hof-external">🔗 External Profile</div>' : ''}
      `;
      
      item.appendChild(img);
      item.appendChild(overlay);
      grid.appendChild(item);
    });
    
    const filterBtns = document.querySelectorAll('.filter-btn');
    filterBtns.forEach(btn => {
      btn.addEventListener('click', function() {
        const year = this.getAttribute('data-year');
        
        filterBtns.forEach(b => b.classList.remove('active'));
        this.classList.add('active');
        
        const items = document.querySelectorAll('.hof-item');
        items.forEach(item => {
          if (year === 'all' || item.getAttribute('data-year') === year) {
            item.style.display = 'flex';
          } else {
            item.style.display = 'none';
          }
        });
      });
    });
  }
</script>

<style>
.hof-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.hof-header {
  text-align: center;
  margin-bottom: 40px;
}

.hof-header h1 {
  font-size: 2.5rem;
  margin-bottom: 10px;
  color: #333;
}

.hof-header p {
  color: #666;
  font-size: 1.1rem;
}

.filter-bar {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 30px;
  position: sticky;
  top: 60px;
  background: white;
  padding: 15px 0;
  z-index: 100;
  border-bottom: 1px solid #dbdbdb;
}

.filter-btn {
  padding: 8px 20px;
  border: 1px solid #dbdbdb;
  background: white;
  border-radius: 30px;
  cursor: pointer;
  font-size: 14px;
  transition: all 0.2s;
}

.filter-btn:hover {
  background: #f5f5f5;
}

.filter-btn.active {
  background: #0095f6;
  color: white;
  border-color: #0095f6;
}

.hof-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 25px;
}

.hof-item {
  position: relative;
  aspect-ratio: 1 / 1;
  overflow: hidden;
  border-radius: 12px;
  cursor: pointer;
  background: #f5f5f5;
  text-decoration: none;
  display: flex;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.hof-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 10px 30px rgba(0,0,0,0.15);
}

.hof-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.hof-item:hover .hof-image {
  transform: scale(1.05);
}

.hof-overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: linear-gradient(to top, rgba(0,0,0,0.8), transparent);
  padding: 30px 15px 15px 15px;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.hof-item:hover .hof-overlay {
  opacity: 1;
}

.hof-title {
  color: white;
  font-size: 16px;
  font-weight: 600;
  margin-bottom: 5px;
  text-align: center;
}

.hof-date {
  color: rgba(255,255,255,0.8);
  font-size: 12px;
  text-align: center;
  margin-bottom: 5px;
}

.hof-external {
  color: #ffd700;
  font-size: 11px;
  text-align: center;
  margin-top: 5px;
}

.no-posts {
  text-align: center;
  padding: 60px;
  color: #666;
  font-size: 18px;
}

.loading {
  text-align: center;
  padding: 40px;
  color: #666;
}

@media (max-width: 768px) {
  .hof-grid {
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 15px;
  }
  
  .filter-btn {
    padding: 6px 15px;
    font-size: 12px;
  }
  
  .hof-header h1 {
    font-size: 2rem;
  }
}
</style>
