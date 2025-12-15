# GraphQL Server Example

A simple GraphQL server built with Apollo Server demonstrating queries, mutations, and relational data structures.

**Based on** 'GraphQL Course for Beginners' by NetNinja (freeCodeCamp.org)

## Overview

This project demonstrates a complete GraphQL API for managing video game reviews. It includes three main entities (Games, Authors, and Reviews) with relationships between them, showcasing GraphQL's ability to handle nested queries and mutations.

## Features

- **Apollo Server 5.0** standalone setup
- **Type-safe GraphQL schema** with custom types and input types
- **Relational data queries** with nested resolvers
- **Full CRUD operations** for games (Create, Read, Update, Delete)
- **In-memory database** for quick prototyping

## Data Model

### Types
- **Game**: Video games with title, platforms, and associated reviews
- **Author**: Review authors with verification status
- **Review**: Game reviews with ratings and content, linked to both games and authors

### Relationships
- Games can have multiple reviews
- Authors can write multiple reviews
- Reviews are linked to both a game and an author

## Installation

```bash
npm install
```

## Usage

Start the server:

```bash
node index.js
```

The GraphQL server will be available at `http://localhost:4000`

## Available Queries

### Fetch All Games
```graphql
query GamesQuery {
  games {
    id
    title
    platforms
  }
}
```

### Fetch Single Game (with nested reviews)
```graphql
query GameQuery($id: ID!) {
  game(id: $id) {
    id
    title
    platforms
    reviews {
      rating
      content
    }
  }
}
```
*Variables:* `{"id": "2"}`

### Fetch Author (with reviews)
```graphql
query AuthorQuery($id: ID!) {
  author(id: $id) {
    id
    name
    verified
    reviews {
      rating
      content
      game {
        title
      }
    }
  }
}
```

### Fetch Review (with nested author and game data)
```graphql
query ReviewQuery($id: ID!) {
  review(id: $id) {
    id
    rating
    content
    author {
      name
      verified
    }
    game {
      title
      platforms
    }
  }
}
```

## Available Mutations

### Add Game
```graphql
mutation AddMutation($game: AddGameType!) {
  addGame(game: $game) {
    id
    title
    platforms
  }
}
```
*Variables:*
```json
{
  "game": {
    "title": "Grand Theft Auto",
    "platforms": ["Xbox", "PS3"]
  }
}
```

### Update Game
```graphql
mutation EditMutation($id: ID!, $edits: EditGameType!) {
  updateGame(id: $id, edits: $edits) {
    id
    title
    platforms
  }
}
```
*Variables:*
```json
{
  "id": "2",
  "edits": {
    "title": "Final Fantasy 7 Remake"
  }
}
```

### Delete Game
```graphql
mutation DeleteMutation($id: ID!) {
  deleteGame(id: $id) {
    id
    title
  }
}
```
*Variables:* `{"id": "1"}`

## Project Structure

```
.
├── index.js           # Apollo Server setup and resolvers
├── schema.js          # GraphQL type definitions
├── _db.js            # In-memory database
├── example_queries/   # Sample queries and mutations
│   ├── games.json
│   ├── game.json
│   ├── author.json
│   ├── review.json
│   ├── add_mutation.json
│   ├── edit_mutation.json
│   └── delete_mutation.json
└── package.json
```

## Technologies

- **@apollo/server** (^5.0.0) - GraphQL server implementation
- **graphql** (^16.11.0) - GraphQL.js reference implementation
- **Node.js** with ES Modules

## Notes

- The server uses an in-memory data store, so all changes are lost when the server restarts
- Port 4000 is used by default
- Example queries are provided in the `example_queries/` directory for reference

## License

ISC
