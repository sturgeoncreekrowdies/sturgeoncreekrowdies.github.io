---
layout: page
icon: fas fa-blog
order: 4
title: Blog
---

<div style="text-align: center; margin-bottom: 2rem;">
  <a href="/categories/">Categories</a> |
  <a href="/tags/">Tags</a> |
  <a href="/archives/">Archives</a>
</div>

{% assign posts = site.posts | sort: 'date' | reverse %}
{% assign posts_per_page = 5 %}

<div id="posts-container">
  {% for post in posts %}
    <article class="post-item" style="margin-bottom: 2rem; padding-bottom: 1rem; border-bottom: 1px solid #eee;">
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      <div>{{ post.date | date: "%B %d, %Y" }}</div>
      <div>{{ post.excerpt | strip_html | truncatewords: 30 }}</div>
    </article>
  {% endfor %}
</div>

<div id="pagination-controls" style="text-align: center; margin-top: 2rem;"></div>

<script>
  const posts = Array.from(document.querySelectorAll('.post-item'));
  const postsPerPage = 5;
  let currentPage = 1;
  
  function showPage(page) {
    const start = (page - 1) * postsPerPage;
    const end = start + postsPerPage;
    
    posts.forEach(post => {
      post.style.display = 'none';
    });
    
    for (let i = start; i < end && i < posts.length; i++) {
      posts[i].style.display = 'block';
    }
    
    updatePaginationButtons();
    
    document.getElementById('posts-container').scrollIntoView({ behavior: 'smooth' });
  }
  
  function updatePaginationButtons() {
    const totalPages = Math.ceil(posts.length / postsPerPage);
    const controls = document.getElementById('pagination-controls');
    
    if (totalPages <= 1) {
      controls.innerHTML = '';
      return;
    }
    
    let html = '<div style="display: inline-block;">';
    
    if (currentPage > 1) {
      html += `<button onclick="changePage(${currentPage - 1})" style="margin-right: 1rem; padding: 0.5rem 1rem; cursor: pointer;">&laquo; Newer</button>`;
    } else {
      html += `<button disabled style="margin-right: 1rem; padding: 0.5rem 1rem; opacity: 0.5;">&laquo; Newer</button>`;
    }
    
    html += `<span style="margin: 0 1rem;">Page ${currentPage} of ${totalPages}</span>`;
    
    if (currentPage < totalPages) {
      html += `<button onclick="changePage(${currentPage + 1})" style="margin-left: 1rem; padding: 0.5rem 1rem; cursor: pointer;">Older &raquo;</button>`;
    } else {
      html += `<button disabled style="margin-left: 1rem; padding: 0.5rem 1rem; opacity: 0.5;">Older &raquo;</button>`;
    }
    
    html += '</div>';
    controls.innerHTML = html;
  }
  
  function changePage(page) {
    currentPage = page;
    showPage(currentPage);
  }
  
  document.addEventListener('DOMContentLoaded', function() {
    showPage(1);
  });
</script>

<style>
  .post-item {
    display: none;
  }
</style>
