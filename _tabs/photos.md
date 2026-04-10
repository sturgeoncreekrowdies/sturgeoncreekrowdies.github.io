---
icon: fas fa-camera
order: 2
title: Photos
permalink: /photos/
---

<div class="gallery-container">
  <div class="filter-bar" id="filter-bar">
    <button class="filter-btn active" data-year="all">All Years</button>
  </div>
  
  <div class="gallery-grid" id="gallery-grid"></div>
</div>

<script>
  // Get all photos from the assets/img/photos directory
  const allPhotos = [
    {% for file in site.static_files %}
      {% if file.path contains 'assets/img/photos' %}
        {% assign filename = file.basename %}
        {% assign year = filename | slice: 0, 4 %}
        {
          url: "{{ file.path | relative_url }}",
          year: "{{ year }}",
          title: "{{ filename | replace: '-', ' ' | replace: '_', ' ' }}"
        },
      {% endif %}
    {% endfor %}
  ];

  console.log('Found photos:', allPhotos.length);
  console.log('Photo data:', allPhotos);

  // Build the gallery
  const gallery = document.getElementById('gallery-grid');
  const filterBar = document.getElementById('filter-bar');
  
  if (allPhotos.length === 0) {
    gallery.innerHTML = '<div class="no-photos">No photos found in assets/img/photos/</div>';
  } else {
    // Get unique years
    const years = [...new Set(allPhotos.map(p => p.year))].sort().reverse();
    
    // Add year buttons
    years.forEach(year => {
      const btn = document.createElement('button');
      btn.className = 'filter-btn';
      btn.setAttribute('data-year', year);
      btn.textContent = year;
      filterBar.appendChild(btn);
    });
    
    // Display all photos
    allPhotos.forEach(photo => {
      const item = document.createElement('div');
      item.className = 'gallery-item';
      item.setAttribute('data-year', photo.year);
      
      const img = document.createElement('img');
      img.src = photo.url;
      img.alt = photo.title;
      img.className = 'gallery-image';
      img.loading = 'lazy';
      
      const overlay = document.createElement('div');
      overlay.className = 'gallery-overlay';
      overlay.innerHTML = `
        <div class="gallery-title">${photo.title}</div>
        <div class="gallery-year">${photo.year}</div>
      `;
      
      item.appendChild(img);
      item.appendChild(overlay);
      gallery.appendChild(item);
    });
    
    // Filter functionality
    const filterBtns = document.querySelectorAll('.filter-btn');
    filterBtns.forEach(btn => {
      btn.addEventListener('click', function() {
        const year = this.getAttribute('data-year');
        
        // Update active button
        filterBtns.forEach(b => b.classList.remove('active'));
        this.classList.add('active');
        
        // Filter items
        const items = document.querySelectorAll('.gallery-item');
        items.forEach(item => {
          if (year === 'all' || item.getAttribute('data-year') === year) {
            item.style.display = 'block';
          } else {
            item.style.display = 'none';
          }
        });
      });
    });
  }
</script>

<style>
.gallery-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
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

.gallery-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
}

.gallery-item {
  position: relative;
  aspect-ratio: 1 / 1;
  overflow: hidden;
  border-radius: 8px;
  cursor: pointer;
  background: #f5f5f5;
}

.gallery-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.gallery-item:hover .gallery-image {
  transform: scale(1.05);
}

.gallery-overlay {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: linear-gradient(to top, rgba(0,0,0,0.7), transparent);
  padding: 20px 10px 10px 10px;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.gallery-item:hover .gallery-overlay {
  opacity: 1;
}

.gallery-title {
  color: white;
  font-size: 14px;
  font-weight: 500;
  margin-bottom: 5px;
  text-align: center;
}

.gallery-year {
  color: rgba(255,255,255,0.8);
  font-size: 12px;
  text-align: center;
}

.no-photos {
  text-align: center;
  padding: 60px;
  color: #666;
  font-size: 18px;
}

@media (max-width: 768px) {
  .gallery-grid {
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    gap: 10px;
  }
  
  .filter-btn {
    padding: 6px 15px;
    font-size: 12px;
  }
}
</style>
