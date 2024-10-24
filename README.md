
# Scriptorium Project Documentation

## Model Design

### `User` Model
```prisma
model User {
  id            Int           @id @default(autoincrement())
  firstName     String
  lastName      String
  email         String        @unique
  passwordHash  String
  avatarUrl     String?
  phoneNumber   String?
  templates     Template[]
  blogPosts     BlogPost[]
  comments      Comment[]
  reports       Report[]
  createdAt     DateTime      @default(now())
  updatedAt     DateTime      @updatedAt
  isAdmin       Boolean       @default(false)
}
```

### `BlogPost` Model
```prisma
model BlogPost {
  id          Int        @id @default(autoincrement())
  title       String
  description String
  tags        Tag[]      @relation("BlogPostTags")
  content     String
  author      User       @relation(fields: [authorId], references: [id])
  authorId    Int
  comments    Comment[]
  upvotes     Int        @default(0)
  downvotes   Int        @default(0)
  createdAt   DateTime   @default(now())
  updatedAt   DateTime   @updatedAt
  isHidden    Boolean    @default(false)
  templates   Template[] @relation("BlogPostTemplates")
}
```

### `Comment` Model
```prisma
model Comment {
  id          Int        @id @default(autoincrement())
  content     String
  author      User       @relation(fields: [authorId], references: [id])
  authorId    Int
  post        BlogPost   @relation(fields: [postId], references: [id])
  postId      Int
  parentId    Int?
  parent      Comment?   @relation("CommentToComment", fields: [parentId], references: [id])
  replies     Comment[]  @relation("CommentToComment")
  upvotes     Int        @default(0)
  downvotes   Int        @default(0)
  createdAt   DateTime   @default(now())
  updatedAt   DateTime   @updatedAt
  isHidden    Boolean    @default(false)
}
```

### `Tag` Model
```prisma
model Tag {
  id        Int         @id @default(autoincrement())
  name      String      @unique
  templates Template[]  @relation("TemplateTags")
  blogPosts BlogPost[]  @relation("BlogPostTags")
}
```

## API Endpoints

### 1. Create a Blog Post
**Endpoint**: `/api/posts`

- **Method**: `POST`
- **Description**: Creates a new blog post.
- **Payload**:
```json
{
  "title": "Post Title",
  "description": "Post description",
  "content": "Content of the blog post",
  "authorId": 1,
  "tags": ["Technology", "Science"]
}
```
- **Response**:
```json
{
  "id": 1,
  "title": "Post Title",
  "description": "Post description",
  "content": "Content of the blog post",
  "authorId": 1,
  "upvotes": 0,
  "downvotes": 0,
  "createdAt": "2024-10-24T12:34:56.789Z",
  "updatedAt": "2024-10-24T12:34:56.789Z"
}
```

### 2. Upvote or Downvote a Blog Post
**Endpoint**: `/api/posts/[postId]/vote`

- **Method**: `POST`
- **Description**: Upvote or downvote a blog post.
- **Payload**:
```json
{
  "voteType": "upvote"  // Use "downvote" to downvote the post
}
```
- **Response**:
```json
{
  "id": 1,
  "title": "Post Title",
  "upvotes": 5,
  "downvotes": 1,
  "updatedAt": "2024-10-24T12:34:56.789Z"
}
```

### 3. Fetch Comments for a Post
**Endpoint**: `/api/comments/[postId]`

- **Method**: `GET`
- **Description**: Retrieves all comments for a specific blog post.
- **Example Request**: `/api/comments/1`
- **Response**:
```json
[
  {
    "id": 1,
    "content": "Great post!",
    "authorId": 2,
    "postId": 1,
    "upvotes": 2,
    "downvotes": 0,
    "replies": [
      {
        "id": 2,
        "content": "I agree!",
        "authorId": 3,
        "upvotes": 0,
        "downvotes": 0
      }
    ]
  }
]
```

### 4. Upvote or Downvote a Comment
**Endpoint**: `/api/comments/[id]/vote`

- **Method**: `POST`
- **Description**: Upvote or downvote a comment.
- **Payload**:
```json
{
  "voteType": "upvote"  // Use "downvote" to downvote the comment
}
```
- **Response**:
```json
{
  "id": 1,
  "content": "Great post!",
  "upvotes": 3,
  "downvotes": 0,
  "updatedAt": "2024-10-24T12:34:56.789Z"
}
```

### 5. Create a Comment for a Blog Post
**Endpoint**: `/api/comments`

- **Method**: `POST`
- **Description**: Adds a new comment to a blog post.
- **Payload**:
```json
{
  "content": "This is a comment",
  "authorId": 1,
  "postId": 1
}
```
- **Response**:
```json
{
  "id": 1,
  "content": "This is a comment",
  "authorId": 1,
  "postId": 1,
  "upvotes": 0,
  "downvotes": 0,
  "createdAt": "2024-10-24T12:34:56.789Z",
  "updatedAt": "2024-10-24T12:34:56.789Z"
}
```

### 6. Get All Blog Posts
**Endpoint**: `/api/posts`

- **Method**: `GET`
- **Description**: Retrieves all blog posts.
- **Response**:
```json
[
  {
    "id": 1,
    "title": "First Post",
    "description": "This is the first blog post.",
    "upvotes": 5,
    "downvotes": 1
  },
  {
    "id": 2,
    "title": "Second Post",
    "description": "This is the second blog post.",
    "upvotes": 3,
    "downvotes": 0
  }
]
```
