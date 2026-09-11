<h2 id="projects" style="margin: 20px 0px 5px;">Projects</h2>
<style>
.project-container {
    display: flex;
    margin-bottom: 30px;
    align-items: flex-start;
}
.project-media {
    flex: 0 0 36%;
    max-width: 36%;
    padding-right: 15px;
    padding-left: 15px;
    position: relative;
}
.project-media img {
    display: block;
    width: 100%;
    height: auto;
    margin: 0 auto;
    object-fit: contain;
    border-radius: 4px;
    cursor: zoom-in;
}
.project-media video {
    width: 100%;
    height: auto;
    object-fit: contain;
    border-radius: 4px;
    background: transparent;
}
.carousel {
    position: relative;
    overflow: hidden;
    touch-action: pan-y;
}
.carousel-track {
    display: flex;
    transition: transform 0.3s ease;
}
.carousel-slide {
    flex: 0 0 100%;
    min-width: 0;
}
.carousel-slide img {
    display: block;
    width: 100%;
    height: auto;
    margin: 0 auto;
    object-fit: contain;
    border-radius: 4px;
    cursor: zoom-in;
}
.carousel-caption {
    font-size: 0.75em;
    color: #888;
    margin: 4px 0 0 0;
    text-align: center;
    font-style: italic;
}
.carousel-dots {
    display: flex;
    justify-content: center;
    gap: 6px;
    margin-top: 8px;
}
.carousel-dot {
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: #ccc;
    cursor: pointer;
    padding: 0;
    border: none;
}
.carousel-dot.active {
    background: rgb(206, 151, 161);
}
@media (max-width: 768px) {
    .project-container {
        flex-direction: column;
        margin-left: 0 !important;
        margin-right: 0 !important;
    }
    .project-media {
        flex: 0 0 auto;
        width: 100%;
        max-width: 480px; /* Limit width on mobile if needed */
    }
}

.lightbox-overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.8);
    z-index: 1000;
    align-items: center;
    justify-content: center;
    cursor: zoom-out;
}
.lightbox-overlay.active {
    display: flex;
}
.lightbox-overlay img {
    max-width: 90vw;
    max-height: 90vh;
    border-radius: 4px;
    box-shadow: 0 4px 30px rgba(0,0,0,0.5);
}
.lightbox-close {
    position: fixed;
    top: 20px;
    right: 30px;
    color: white;
    font-size: 2.5rem;
    cursor: pointer;
    line-height: 1;
}
.lightbox-nav {
    display: none;
    position: fixed;
    top: 50%;
    transform: translateY(-50%);
    background: rgba(255,255,255,0.15);
    border: none;
    color: white;
    font-size: 1.5rem;
    width: 48px;
    height: 48px;
    border-radius: 50%;
    cursor: pointer;
    align-items: center;
    justify-content: center;
    z-index: 1001;
}
.lightbox-nav:hover {
    background: rgba(255,255,255,0.3);
}
.lightbox-nav.show {
    display: flex;
}
.lightbox-prev {
    left: 20px;
}
.lightbox-next {
    right: 20px;
}
</style>

<div class="projects">
    {% for project in site.data.projects %}
    <div class="project-container" {% if project.ongoing %}style="background-color: rgba(255, 121, 0, 0.05); padding: 20px 60px 20px 20px; border-radius: 10px; margin-left: -20px; margin-right: -40px;"{% endif %}>
        <div class="project-media">
            {% if project.video %}
                <video width="100%" autoplay loop muted playsinline style="border-radius: 4px;">
                    {% assign file_ext = project.video | split: '.' | last | downcase %}
                    {% if file_ext == 'mov' %}
                        <source src="{{ project.video }}" type="video/quicktime">
                    {% elsif file_ext == 'webm' %}
                        <source src="{{ project.video }}" type="video/webm">
                    {% else %}
                        <source src="{{ project.video }}" type="video/mp4">
                    {% endif %}
                </video>
            {% elsif project.images %}
                <div class="carousel">
                    <div class="carousel-track">
                        {% for img in project.images %}
                        <div class="carousel-slide">
                            <img class="zoomable" src="{{ img.src }}" alt="{{ project.title }}">
                            {% if img.caption %}<p class="carousel-caption">{{ img.caption }}</p>{% endif %}
                        </div>
                        {% endfor %}
                    </div>
                    {% if project.images.size > 1 %}
                    <div class="carousel-dots">
                        {% for img in project.images %}
                        <button class="carousel-dot{% if forloop.first %} active{% endif %}" aria-label="Go to image {{ forloop.index }}"></button>
                        {% endfor %}
                    </div>
                    {% endif %}
                </div>
            {% elsif project.image %}
                {% assign file_ext = project.image | split: '.' | last | downcase %}
                {% if file_ext == 'mp4' or file_ext == 'mov' or file_ext == 'webm' %}
                    <video width="100%" autoplay loop muted playsinline style="border-radius: 4px;">
                        {% if file_ext == 'mov' %}
                            <source src="{{ project.image }}" type="video/quicktime">
                        {% elsif file_ext == 'webm' %}
                            <source src="{{ project.image }}" type="video/webm">
                        {% else %}
                            <source src="{{ project.image }}" type="video/mp4">
                        {% endif %}
                    </video>
                {% else %}
                    <img class="zoomable" src="{{ project.image }}" alt="{{ project.title }}" style="width: {{ project.image_scale | default: 100 }}%;">
                {% endif %}
            {% endif %}
            {% if project.caption %}
                <p style="font-size: 0.75em; color: #888; margin: 4px 0 0 0; text-align: center; font-style: italic;">{{ project.caption }}</p>
            {% endif %}
        </div>
        <div class="project-content" style="flex: 0 0 64%; max-width: 64%; padding-right: 15px; padding-left: 20px;">
            <div class="title" style="font-weight: bolder; font-size: 1.1em;"><a style="color: inherit; text-decoration: none;">{{ project.title }}</a></div>
            <div class="author" style="margin-top: 2px;">{{ project.date }}{% if project.institution %} | {% if project.institution_url %}<a href="{{ project.institution_url }}" target="_blank" rel="noopener noreferrer">{{ project.institution }}</a>{% else %}{{ project.institution }}{% endif %}{% endif %}{% if project.link_url %} | <a href="{{ project.link_url }}" target="_blank" rel="noopener noreferrer">{{ project.link_text | default: project.link_url }}</a>{% endif %}</div>
            <div class="periodical" style="margin-top: 5px; font-size: 0.95em;">{{ project.description }}</div>
        </div>
    </div>
    {% endfor %}
</div>

<div id="lightbox-overlay" class="lightbox-overlay">
  <span class="lightbox-close">&times;</span>
  <button id="lightbox-prev" class="lightbox-nav lightbox-prev" aria-label="Previous image">&#10094;</button>
  <img id="lightbox-img" src="" alt="">
  <button id="lightbox-next" class="lightbox-nav lightbox-next" aria-label="Next image">&#10095;</button>
</div>

<script>
document.querySelectorAll('.carousel').forEach(function(carousel) {
  var track = carousel.querySelector('.carousel-track');
  var slides = carousel.querySelectorAll('.carousel-slide');
  var dots = carousel.querySelectorAll('.carousel-dot');
  var index = 0;

  function goTo(i) {
    index = Math.max(0, Math.min(slides.length - 1, i));
    track.style.transform = 'translateX(-' + (index * 100) + '%)';
    dots.forEach(function(dot, di) { dot.classList.toggle('active', di === index); });
  }

  carousel._goTo = goTo;

  dots.forEach(function(dot, di) {
    dot.addEventListener('click', function() { goTo(di); });
  });

  var touchStartX = 0;
  var touchStartY = 0;
  carousel.addEventListener('touchstart', function(e) {
    touchStartX = e.touches[0].clientX;
    touchStartY = e.touches[0].clientY;
  }, { passive: true });
  carousel.addEventListener('touchend', function(e) {
    var dx = e.changedTouches[0].clientX - touchStartX;
    var dy = e.changedTouches[0].clientY - touchStartY;
    if (Math.abs(dx) > 40 && Math.abs(dx) > Math.abs(dy)) {
      goTo(dx < 0 ? index + 1 : index - 1);
    }
  });

  var wheelCooldown = false;
  carousel.addEventListener('wheel', function(e) {
    if (Math.abs(e.deltaX) > Math.abs(e.deltaY) && Math.abs(e.deltaX) > 10) {
      e.preventDefault();
      if (wheelCooldown) return;
      wheelCooldown = true;
      goTo(e.deltaX > 0 ? index + 1 : index - 1);
      setTimeout(function() { wheelCooldown = false; }, 400);
    }
  }, { passive: false });
});

var overlay = document.getElementById('lightbox-overlay');
var lightboxImg = document.getElementById('lightbox-img');
var prevBtn = document.getElementById('lightbox-prev');
var nextBtn = document.getElementById('lightbox-next');
var lbCarousel = null;
var lbImages = [];
var lbIndex = 0;

function showLightboxSlide(i) {
  lbIndex = Math.max(0, Math.min(lbImages.length - 1, i));
  var img = lbImages[lbIndex];
  lightboxImg.src = img.src;
  lightboxImg.alt = img.alt;
  if (lbCarousel && lbCarousel._goTo) lbCarousel._goTo(lbIndex);
  var hasNav = lbImages.length > 1;
  prevBtn.classList.toggle('show', hasNav);
  nextBtn.classList.toggle('show', hasNav);
}

document.querySelectorAll('.project-media img.zoomable').forEach(function(img) {
  img.addEventListener('click', function() {
    var carousel = img.closest('.carousel');
    if (carousel) {
      lbCarousel = carousel;
      lbImages = Array.prototype.slice.call(carousel.querySelectorAll('img.zoomable'));
    } else {
      lbCarousel = null;
      lbImages = [img];
    }
    showLightboxSlide(lbImages.indexOf(img));
    overlay.classList.add('active');
  });
});

prevBtn.addEventListener('click', function(e) {
  e.stopPropagation();
  showLightboxSlide(lbIndex - 1);
});
nextBtn.addEventListener('click', function(e) {
  e.stopPropagation();
  showLightboxSlide(lbIndex + 1);
});

overlay.addEventListener('click', function() {
  overlay.classList.remove('active');
});
document.addEventListener('keydown', function(e) {
  if (!overlay.classList.contains('active')) return;
  if (e.key === 'Escape') overlay.classList.remove('active');
  if (e.key === 'ArrowLeft') showLightboxSlide(lbIndex - 1);
  if (e.key === 'ArrowRight') showLightboxSlide(lbIndex + 1);
});

var lbTouchStartX = 0;
var lbTouchStartY = 0;
overlay.addEventListener('touchstart', function(e) {
  lbTouchStartX = e.touches[0].clientX;
  lbTouchStartY = e.touches[0].clientY;
}, { passive: true });
overlay.addEventListener('touchend', function(e) {
  var dx = e.changedTouches[0].clientX - lbTouchStartX;
  var dy = e.changedTouches[0].clientY - lbTouchStartY;
  if (Math.abs(dx) > 40 && Math.abs(dx) > Math.abs(dy)) {
    showLightboxSlide(dx < 0 ? lbIndex + 1 : lbIndex - 1);
  }
});

var lbWheelCooldown = false;
overlay.addEventListener('wheel', function(e) {
  if (Math.abs(e.deltaX) > Math.abs(e.deltaY) && Math.abs(e.deltaX) > 10) {
    e.preventDefault();
    if (lbWheelCooldown) return;
    lbWheelCooldown = true;
    showLightboxSlide(e.deltaX > 0 ? lbIndex + 1 : lbIndex - 1);
    setTimeout(function() { lbWheelCooldown = false; }, 400);
  }
}, { passive: false });
</script>