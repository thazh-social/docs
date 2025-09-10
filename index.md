# Thazh Social Mobile API Documentation

## Base URL
```
https://thazh-social.alwaysdata.net/api/mobile/
```

## Authentication
All authenticated endpoints require a Bearer token in the Authorization header:
```
Authorization: Bearer YOUR_TOKEN_HERE
```

## Response Format
All responses follow this format:
```json
{
  "status": "success|error",
  "message": "Response message",
  "data": {},
  "timestamp": 1234567890
}
```

---

## 🔐 Authentication Endpoints

### Login
**POST** `/auth/login`

Request:
```json
{
  "email": "user@example.com",
  "password": "password123"
}
```

Response:
```json
{
  "status": "success",
  "message": "Login successful",
  "data": {
    "token": "abc123...",
    "expires_at": "2024-01-01 00:00:00",
    "user": {
      "id": 1,
      "username": "johndoe",
      "email": "user@example.com",
      "full_name": "John Doe",
      "avatar": "uploads/avatars/avatar_1.jpg"
    }
  }
}
```

### Register
**POST** `/auth/register`

Request:
```json
{
  "username": "johndoe",
  "email": "user@example.com",
  "password": "password123",
  "full_name": "John Doe"
}
```

### Logout
**POST** `/auth/logout`
*Requires Authentication*

### Refresh Token
**POST** `/auth/refresh`
*Requires Authentication*

---

## 👤 User Endpoints

### Get User Profile
**GET** `/user/profile/{user_id?}`
*Requires Authentication*

If no user_id provided, returns current user's profile.

Response:
```json
{
  "data": {
    "id": 1,
    "username": "johndoe",
    "full_name": "John Doe",
    "bio": "Hello world!",
    "avatar": "uploads/avatars/avatar_1.jpg",
    "followers_count": 150,
    "following_count": 200,
    "posts_count": 45,
    "active_stories_count": 2,
    "is_following": true
  }
}
```

### Update Profile
**PUT** `/user/update`
*Requires Authentication*

Request:
```json
{
  "full_name": "John Smith",
  "bio": "Updated bio"
}
```

### Follow User
**POST** `/user/follow/{user_id}`
*Requires Authentication*

### Unfollow User
**DELETE** `/user/unfollow/{user_id}`
*Requires Authentication*

### Get Followers
**GET** `/user/followers/{user_id}`

### Get Following
**GET** `/user/following/{user_id}`

---

## 📝 Posts Endpoints

### Get Feed
**GET** `/posts/feed?page=1&limit=20`
*Requires Authentication*

Returns posts from followed users and own posts.

Response:
```json
{
  "data": {
    "posts": [
      {
        "id": 1,
        "content": "Hello world! #first",
        "image": "uploads/posts/post_1.jpg",
        "likes_count": 10,
        "comments_count": 5,
        "user_liked": true,
        "created_at": "2024-01-01 12:00:00",
        "username": "johndoe",
        "full_name": "John Doe",
        "avatar": "uploads/avatars/avatar_1.jpg",
        "is_verified": false
      }
    ],
    "page": 1,
    "has_more": true
  }
}
```

### Get Explore Posts
**GET** `/posts/explore?page=1&limit=20`

Returns popular public posts.

### Get Post Details
**GET** `/posts/detail/{post_id}`

Response includes post data and comments.

### Create Post
**POST** `/posts/create`
*Requires Authentication*

Request:
```json
{
  "content": "Hello world! #hashtag",
  "image": "uploads/posts/image.jpg",
  "is_public": true
}
```

### Get User Posts
**GET** `/posts/user/{user_id}?page=1&limit=20`

---

## ❤️ Interactions Endpoints

### Like Post
**POST** `/interactions/like/{post_id}`
*Requires Authentication*

### Unlike Post
**DELETE** `/interactions/unlike/{post_id}`
*Requires Authentication*

### Add Comment
**POST** `/interactions/comment/{post_id}`
*Requires Authentication*

Request:
```json
{
  "content": "Great post!",
  "image": "uploads/comments/comment_image.jpg"
}
```

---

## 📸 Stories Endpoints

### Get Stories Feed
**GET** `/stories/feed`
*Requires Authentication*

Response:
```json
{
  "data": [
    {
      "user_id": 1,
      "username": "johndoe",
      "full_name": "John Doe",
      "avatar": "uploads/avatars/avatar_1.jpg",
      "is_verified": false,
      "story_count": 3,
      "story_ids": [1, 2, 3],
      "latest_story": "2024-01-01 12:00:00"
    }
  ]
}
```

### View Story
**GET** `/stories/view/{story_id}`
*Requires Authentication*

Automatically records view and sends notification.

Response:
```json
{
  "data": {
    "id": 1,
    "content_type": "image",
    "media_url": "uploads/stories/story_1.jpg",
    "text_content": null,
    "background_color": "#000000",
    "views_count": 25,
    "created_at": "2024-01-01 12:00:00",
    "username": "johndoe",
    "full_name": "John Doe",
    "avatar": "uploads/avatars/avatar_1.jpg"
  }
}
```

### Create Story
**POST** `/stories/create`
*Requires Authentication*

Request:
```json
{
  "content_type": "text",
  "text_content": "Hello from story!",
  "background_color": "#FF6B6B",
  "text_color": "#FFFFFF",
  "font_family": "Arial",
  "font_size": 24,
  "text_alignment": "center",
  "duration": 15,
  "privacy_type": "followers"
}
```

For image/video stories:
```json
{
  "content_type": "image",
  "media_url": "uploads/stories/story.jpg",
  "duration": 15,
  "privacy_type": "public"
}
```

---

## 🔍 Search Endpoints

### Search All
**GET** `/search/all?q=keyword&limit=10`

Returns users, posts, and hashtags.

### Search Users
**GET** `/search/users?q=keyword&limit=10`

### Search Hashtags
**GET** `/search/hashtags?q=keyword&limit=10`

---

## 🔔 Notifications Endpoints

### Get Notifications
**GET** `/notifications/list?page=1&limit=20`
*Requires Authentication*

Response:
```json
{
  "data": {
    "notifications": [
      {
        "id": 1,
        "type": "like",
        "is_read": false,
        "created_at": "2024-01-01 12:00:00",
        "actor_username": "jane",
        "actor_full_name": "Jane Doe",
        "actor_avatar": "uploads/avatars/avatar_2.jpg",
        "actor_is_verified": true,
        "post_content": "My awesome post!",
        "post_image": "uploads/posts/post_1.jpg"
      }
    ],
    "page": 1,
    "has_more": true
  }
}
```

### Get Unread Count
**GET** `/notifications/count`
*Requires Authentication*

### Mark as Read
**PUT** `/notifications/mark_read/{notification_id?}`
*Requires Authentication*

If no notification_id provided, marks all as read.

---

## 📤 Upload Endpoints

### Upload Avatar
**POST** `/upload/avatar`
*Requires Authentication*

Form data with 'file' field containing image.

### Upload Post Image
**POST** `/upload/post`
*Requires Authentication*

### Upload Story Media
**POST** `/upload/story`
*Requires Authentication*

Response:
```json
{
  "data": {
    "url": "https://your-domain.com/uploads/posts/image.jpg",
    "path": "uploads/posts/image.jpg",
    "filename": "post_1_1642678800.jpg"
  }
}
```

---

## 📱 Android Implementation Examples

### Login Example (Kotlin)
```kotlin
data class LoginRequest(
    val email: String,
    val password: String
)

data class LoginResponse(
    val status: String,
    val message: String,
    val data: LoginData
)

data class LoginData(
    val token: String,
    val expires_at: String,
    val user: User
)

// Retrofit interface
interface ApiService {
    @POST("auth/login")
    suspend fun login(@Body request: LoginRequest): LoginResponse
    
    @GET("posts/feed")
    suspend fun getFeed(
        @Header("Authorization") token: String,
        @Query("page") page: Int = 1,
        @Query("limit") limit: Int = 20
    ): PostsResponse
}

// Usage
class Repository {
    suspend fun login(email: String, password: String): Result<LoginData> {
        return try {
            val response = apiService.login(LoginRequest(email, password))
            if (response.status == "success") {
                Result.success(response.data)
            } else {
                Result.failure(Exception(response.message))
            }
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
}
```

### Image Upload Example (Kotlin)
```kotlin
fun uploadImage(file: File, type: String): Result<String> {
    val requestFile = file.asRequestBody("image/*".toMediaTypeOrNull())
    val body = MultipartBody.Part.createFormData("file", file.name, requestFile)
    
    return try {
        val response = apiService.uploadFile("Bearer $token", type, body)
        if (response.status == "success") {
            Result.success(response.data.url)
        } else {
            Result.failure(Exception(response.message))
        }
    } catch (e: Exception) {
        Result.failure(e)
    }
}
```

---

## 🔧 Error Codes

| Code | Message | Description |
|------|---------|-------------|
| 400 | Bad Request | Invalid request parameters |
| 401 | Unauthorized | Invalid or missing authentication |
| 403 | Forbidden | Access denied |
| 404 | Not Found | Resource not found |
| 405 | Method Not Allowed | HTTP method not supported |
| 500 | Internal Server Error | Server error |

---

## 📋 Rate Limiting

- **Authentication endpoints**: 5 requests per minute
- **General endpoints**: 100 requests per minute per user
- **Upload endpoints**: 10 requests per minute per user

## 🔄 Pagination

Most list endpoints support pagination:
- `page`: Page number (default: 1)
- `limit`: Items per page (default: 20, max: 50)
- Response includes `has_more` boolean to indicate if more pages exist

## 📝 Notes

1. All timestamps are in UTC format
2. File uploads must be multipart/form-data
3. Image uploads are automatically resized and optimized
4. Stories automatically expire after 24 hours
5. Notifications are created automatically for interactions
6. Search queries must be at least 2 characters long
