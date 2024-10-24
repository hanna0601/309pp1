
# Scriptorium Project Documentation

## Model Design

### `User` Model
```prisma

```

### `BlogPost` Model
```prisma

```

### `Comment` Model
```prisma

```

### `Tag` Model
```prisma

```

## API Endpoints

### Blog Posts


### Endpoint: `/api/comments/`
- **Description**: Create a new comment on a blog post
- **URL**: `http://localhost:3000/api/comments`
- **Method**: `POST`
- **Payload**:
```json
{
  "content": "Great post",
  "authorId": 1,
  "postId": 2
}
```
- **Response**:
```json
{
    "id": 1,
    "content": "Great post",
    "authorId": 1,
    "postId": 2,
    "parentId": null,
    "upvotes": 0,
    "downvotes": 0,
    "createdAt": "2024-10-24T21:53:33.470Z",
    "updatedAt": "2024-10-24T21:53:33.470Z",
    "isHidden": false
}
```

### Endpoint: `/api/comments/`
- **Description**: Reply to an existing comment on a blog post
- **URL**: `http://localhost:3000/api/comments`
- **Method**: `POST`
- **Payload**:
```json
{
  "content": "Thank you",
  "authorId": 2,
  "postId": 2,
  "parentId": 1
}
```
- **Response**:
```json
{
    "id": 2,
    "content": "Thank you",
    "authorId": 2,
    "postId": 2,
    "parentId": 1,
    "upvotes": 0,
    "downvotes": 0,
    "createdAt": "2024-10-24T21:55:34.909Z",
    "updatedAt": "2024-10-24T21:55:34.909Z",
    "isHidden": false
}
```

### Endpoint: `/api/posts/[id]/vote`
- **Description**: Upvote or downvote a blog post
- **URL**: `http://localhost:3000/api/posts/1/vote`
- **Method**: `POST`
- **Payload**:
```json
{
  "voteType": "downvote"  // or "upvote" for downvoting
}
```
- **Response**:
```json
{
    "id": 1,
    "title": "First Post",
    "description": "This is the first blog post.",
    "content": "Hello, this is a post about technology!",
    "authorId": 1,
    "upvotes": 0,
    "downvotes": 1,
    "createdAt": "2024-10-24T22:10:43.515Z",
    "updatedAt": "2024-10-24T22:11:58.265Z",
    "isHidden": false
}
```

### Endpoint: `/api/comments/[id]/vote`
- **Description**: Upvote or downvote a comment
- **URL**: `http://localhost:3000/api/comments/2/vote`
- **Method**: `POST`
- **Payload**:
```json
{
  "voteType": "upvote"  // or "upvote" for downvoting
}
```
- **Response**:
```json
{
    "id": 2,
    "content": "Thank you",
    "authorId": 2,
    "postId": 2,
    "parentId": 1,
    "upvotes": 1,
    "downvotes": 0,
    "createdAt": "2024-10-24T22:10:58.924Z",
    "updatedAt": "2024-10-24T22:15:09.343Z",
    "isHidden": false
}
```



