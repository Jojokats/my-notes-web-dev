## Hero image:

<body class="blog">

<section class="feature-image feature-image-default-alt" data-type="background" data-speed="2">
  <h1 class="page-title">Blog</h1>
</section>



## CSS:

.feature-image: {
  display: table;
  width: 100%;
}

.feature-image-default-alt {
  background: url('../img/hipster.jpg') no-repeat;
  background-size: cover;
}

.feature-image h1 {
  display: table-cell;
  vertical-align: middle;
  text-align: center;
  color: white;
}


### Post comment badge styling:

.post-comments-badge {
  height: 70px;
  width: 70px;
  position: absolute;
  top: 25px;
  right: 20px;
  border: none;
  border-radius: 100%;
  background: #79b044;
  text-align: center;
  display: table;
}

.post-comments-badge a {
  display: table-cell;
  vertical-align: middle;
  color: #fff;
  font-size: 20px;
  line-height: 20px;
}
