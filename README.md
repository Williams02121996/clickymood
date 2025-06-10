<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Light Book</title>
    <!-- Font Awesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <style>
        /* --- General & Reset Styles --- */
        body {
            margin: 0;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: #18191A;
            color: #E4E6EB;
        }

        a {
            text-decoration: none;
            color: inherit;
        }

        /* --- Header Styles --- */
        .main-header {
            background-color: #242526;
            border-bottom: 1px solid #393A3B;
            padding: 0 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: sticky;
            top: 0;
            z-index: 100;
            height: 56px;
        }
        
        .header-left, .header-center, .header-right {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .header-center {
            flex-grow: 1;
            justify-content: center;
        }

        .header-icon {
            font-size: 24px;
            color: #B0B3B8;
            padding: 10px 25px;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.2s;
        }
        
        .header-icon.active {
            color: #2D88FF;
            border-bottom: 3px solid #2D88FF;
            border-radius: 0;
            padding-bottom: 7px;
        }

        .header-icon:not(.active):hover {
            background-color: #3A3B3C;
        }
        
        .header-right .header-icon {
            background-color: #3A3B3C;
            border-radius: 50%;
            padding: 8px;
            font-size: 20px;
        }

        .fb-logo {
            font-size: 28px;
            color: #2D88FF;
        }
        
        /* --- Main Feed Container --- */
        .feed-container {
            max-width: 680px;
            margin: 20px auto;
            padding: 0 10px;
        }

        /* --- Post Styles --- */
        .post {
            background-color: #242526;
            border-radius: 8px;
            margin-bottom: 15px;
            border: 1px solid #393A3B;
        }

        .post-header {
            display: flex;
            align-items: center;
            padding: 12px 16px;
        }

        .avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            margin-right: 12px;
            object-fit: cover;
        }

        .post-info {
            flex-grow: 1;
        }
        
        .post-author {
            font-weight: 600;
            margin: 0;
            font-size: 0.9rem;
        }
        
        .post-meta {
            margin: 0;
            font-size: 0.8rem;
            color: #B0B3B8;
        }

        .follow-btn {
            color: #4599FF;
            font-weight: 600;
            cursor: pointer;
            font-size: 0.9rem;
        }

        .post-options {
            color: #B0B3B8;
            cursor: pointer;
            font-size: 20px;
            padding: 8px;
        }

        .post-content {
            padding: 4px 16px 12px 16px;
            font-size: 0.95rem;
            line-height: 1.4;
        }
        
        .post-content .hashtag {
            color: #4599FF;
            font-weight: 500;
        }

        .post-media {
            position: relative;
            background-color: #000;
        }

        .media-preview {
            width: 100%;
            display: block;
            max-height: 70vh;
            object-fit: contain;
        }
        
        .live-badge {
            position: absolute;
            top: 12px;
            left: 12px;
            background-color: #FA383E;
            color: white;
            padding: 3px 8px;
            font-size: 0.8rem;
            font-weight: 600;
            border-radius: 4px;
            text-transform: uppercase;
        }

        .play-button {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 60px;
            color: rgba(255, 255, 255, 0.8);
            background-color: rgba(0, 0, 0, 0.4);
            border-radius: 50%;
            width: 90px;
            height: 90px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: background-color 0.2s;
        }

        .play-button:hover {
            background-color: rgba(0, 0, 0, 0.6);
        }
        
        .play-button i {
            margin-left: 8px; /* Optical centering for play icon */
        }
        
        .post-stats {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 16px;
            font-size: 0.9rem;
            color: #B0B3B8;
        }

        .reactions i {
            color: #4599FF;
        }

        .post-actions {
            display: flex;
            justify-content: space-around;
            padding: 4px 16px 12px 16px;
            border-top: 1px solid #3A3B3C;
        }

        .action-button {
            flex-grow: 1;
            text-align: center;
            padding: 8px;
            border-radius: 6px;
            cursor: pointer;
            transition: background-color 0.2s;
            color: #B0B3B8;
            font-weight: 600;
        }
        
        .action-button:hover {
            background-color: #3A3B3C;
        }
        
        .action-button i {
            margin-right: 8px;
            font-size: 18px;
        }
        
        /* --- Mobile Responsive --- */
        @media (max-width: 768px) {
            .header-center {
                display: none; /* Hide center icons on smaller screens */
            }
            .header-left {
                flex-grow: 1;
            }
            .main-header {
                padding: 0 10px;
            }
            .feed-container {
                margin-top: 10px;
            }
            .post {
                border-radius: 0;
                border-left: none;
                border-right: none;
            }
        }
    </style>
</head>
<body>

    <!-- Header Navigation -->
    <header class="main-header">
        <div class="header-left">
            <i class="fa-brands fa-facebook fb-logo"></i>
        </div>
        <div class="header-center">
            <a href="#"><i class="fas fa-home header-icon"></i></a>
            <a href="#"><i class="fas fa-tv header-icon active"></i></a>
            <a href="#"><i class="fas fa-store header-icon"></i></a>
            <a href="#"><i class="fas fa-users header-icon"></i></a>
        </div>
        <div class="header-right">
            <a href="#"><i class="fas fa-search header-icon"></i></a>
            <a href="#"><i class="fab fa-facebook-messenger header-icon"></i></a>
            <a href="#"><i class="fas fa-bell header-icon"></i></a>
        </div>
    </header>

    <!-- Main Content Feed -->
    <main class="feed-container">
        

      
      <!-- === POST 1 === (You can copy and paste this whole block to create more posts) -->
<article class="post">
    <div class="post-header">
        <img src="https://dn721602.ca.archive.org/0/items/youtube-RJ1Qcri1Cco/__ia_thumb.jpg" alt="User Avatar" class="avatar">
        <div class="post-info">
            <h3 class="post-author">
                <a href="#">Alysa Gap</a> · <span class="follow-btn">Follow</span>
            </h3>
            <p class="post-meta">Suggested for you · 12 m · <i class="fas fa-globe-americas"></i></p>
        </div>
        <i class="fas fa-ellipsis-h post-options"></i>
    </div>
    <div class="post-content">
        <p>This is my first post on this awesome new blog! What a great site. <br> 
        <span class="hashtag">#NewBlog</span> <span class="hashtag">#BloggerTheme</span> <span class="hashtag">#Design</span></p>
    </div>
    <div class="post-media">
        <video class="media-preview" controls poster="https://dn721602.ca.archive.org/0/items/youtube-RJ1Qcri1Cco/__ia_thumb.jpg">
            <source src="https://archive.org/download/youtube-RJ1Qcri1Cco/RJ1Qcri1Cco.mp4" type="video/mp4">
            Your browser does not support the video tag.
        </video>
    </div>
    <div class="post-stats">
        <div class="reactions">
            <i class="fas fa-thumbs-up"></i> 
            <span id="like-count">19524</span>
        </div>
        <div class="comments-shares">
            2.4K Comments · 1.3K Shares · 5.2M Views
        </div>
    </div>

    <div class="post-actions">
        <div class="action-button" onclick="addLike()">
            <i class="far fa-thumbs-up"></i> Like
        </div>

        <a href="https://gmail.com" target="_blank" rel="noopener noreferrer" class="action-button" style="text-decoration: none; color: inherit;">
            <i class="far fa-comment-alt"></i> Comment
        </a>

        <a href="https://yahoo.com" target="_blank" rel="noopener noreferrer" class="action-button" style="text-decoration: none; color: inherit;">
          
            <i class="fas fa-share"></i> Share
        </a>
    </div>
</article>

<script>
    const likeKey = 'likes-post-1';
    let likeCount = localStorage.getItem(likeKey) || 157000;
    document.getElementById("like-count").innerText = likeCount;

    function addLike() {
        likeCount++;
        document.getElementById("like-count").innerText = likeCount;
        localStorage.setItem(likeKey, likeCount);
    }
</script>
<!-- === END POST 1 === -->
      
<!-- === POST 2 === -->
<article class="post">
    <div class="post-header">
        <img src="https://dn721602.ca.archive.org/0/items/youtube-RJ1Qcri1Cco/__ia_thumb.jpg" alt="User Avatar" class="avatar">
        <div class="post-info">
            <h3 class="post-author">
                <a href="https://google.com">Alina Nicole</a> · <span class="follow-btn">Follow</span>
            </h3>
            <p class="post-meta">Suggested for you · 12 m · <i class="fas fa-globe-americas"></i></p>
        </div>
        <i class="fas fa-ellipsis-h post-options"></i>
    </div>

    <div class="post-content">
        <p>ReneesRealm ASMR Ear Licking 3Dio <br> 
        <span class="hashtag">#NewBlog</span> <span class="hashtag">#ReneesRealm</span> <span class="hashtag">#Licking</span></p>
    </div>

    <div class="post-media" style="position: relative;">
        <img src="https://dn721803.ca.archive.org/0/items/reneesrealm-20220317/__ia_thumb.jpg" alt="Media preview" class="media-preview" style="width: 100%; display: block;">
        <div class="live-badge">Live</div>
        <a href="https://google.com" target="_blank" rel="noopener noreferrer"
           style="position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);">
            <div class="play-button" style="font-size: 36px; background: rgba(0,0,0,0.6); padding: 12px 18px; border-radius: 50%; color: white;">
                <i class="fas fa-play"></i>
            </div>
        </a>
    </div>

    <div class="post-stats">
        <div class="reactions">
            <i class="fas fa-thumbs-up"></i> 
            <span id="like-count">15708</span>
        </div>
        <div class="comments-shares">
            1.1K Comments · 2.5K Shares · 9.9M Views
        </div>
    </div>

    <div class="post-actions">
        <div class="action-button" onclick="addLike()">
            <i class="far fa-thumbs-up"></i> Like
        </div>

        <a href="https://gmail.com" target="_blank" rel="noopener noreferrer" class="action-button" style="text-decoration: none; color: inherit;">
            <i class="far fa-comment-alt"></i> Comment
        </a>

        <a href="https://yahoo.com" target="_blank" rel="noopener noreferrer" class="action-button" style="text-decoration: none; color: inherit;">
          
            <i class="fas fa-share"></i> Share
        </a>
    </div>
</article>

<script>
    const likeKey = 'likes-post-1';
    let likeCount = localStorage.getItem(likeKey) || 157000;
    document.getElementById("like-count").innerText = likeCount;

    function addLike() {
        likeCount++;
        document.getElementById("like-count").innerText = likeCount;
        localStorage.setItem(likeKey, likeCount);
    }
</script>
<!-- === END POST 2 === -->
      
      

        <!-- Add more posts by copying the post block above -->

    </main>

</body>
</html>
