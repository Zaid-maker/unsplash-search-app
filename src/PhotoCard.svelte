<script>
  export let photo;
</script>

<li class="photo-card">
  <a href={photo.links.html} rel="noopener noreferrer" target="_blank">
    <img src={photo.urls.small} alt={photo.description || ""} class="photo" />
  </a>
  <span class="photographer">
    <a href={photo.user.links.html} rel="noopener noreferrer">
      {photo.user.name}
    </a>
  </span>
</li>

<style>
  .photo-card {
    animation: show 0.5s forwards ease-in-out;
  }

  .photo {
    width: 100%;
    object-fit: cover;
    height: 300px;
    background-color: #ccc;
    margin-bottom: 10px;
  }

  .photographer {
    display: inline-block;
  }

  .photographer a {
    color: #333;
    text-decoration: none;
  }

  .photographer a:hover {
    text-decoration: underline;
  }

  @keyframes show {
    0% {
      opacity: 0;
      transform: translateY(100px);
    }

    100% {
      opacity: 1;
      transform: translateY(0);
    }
  }
</style>
