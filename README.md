# BlogAggregator

A command-line RSS feed aggregator that allows you to follow multiple blogs and view their latest posts in one place.

## Prerequisites

- Node.js v22.15.0 (see [.nvmrc](.nvmrc))
- PostgreSQL database
- npm or yarn package manager

## Installation

1. Clone the repository:
```bash
git clone <your-repo-url>
cd BlogAggregator
```

2. Install dependencies:
```bash
npm install
```

3. Set up your configuration file at `~/.gatorconfig.json`:
```json
{
  "db_url": "postgresql://username:password@localhost:5432/dbname",
  "current_user_name": ""
}
```

Replace the `db_url` with your actual PostgreSQL connection string.

4. Run database migrations:
```bash
npm run migrate
```

## Usage

Run commands using:
```bash
npm start -- <command> [args...]
```

### Available Commands

#### User Management

- **Register a new user:**
  ```bash
  npm start -- register <username>
  ```
  Creates a new user and automatically logs you in.

- **Login as an existing user:**
  ```bash
  npm start -- login <username>
  ```

- **List all users:**
  ```bash
  npm start -- users
  ```

#### Feed Management

- **Add a new RSS feed:**
  ```bash
  npm start -- addfeed <feed_name> <feed_url>
  ```
  Adds a feed and automatically follows it.

- **List all feeds:**
  ```bash
  npm start -- feeds
  ```

- **Follow a feed:**
  ```bash
  npm start -- follow <feed_url>
  ```

- **Unfollow a feed:**
  ```bash
  npm start -- unfollow <feed_url>
  ```

- **List feeds you're following:**
  ```bash
  npm start -- following
  ```

#### Reading Posts

- **Browse posts from your followed feeds:**
  ```bash
  npm start -- browse [limit]
  ```
  Shows the latest posts from feeds you follow. Default limit is 2.

- **Aggregate feeds (continuous scraping):**
  ```bash
  npm start -- agg <time_between_reqs>
  ```
  Continuously fetches new posts from feeds. Time format: `1h`, `30m`, `15s`, or `3500ms`.
  
  Example:
  ```bash
  npm start -- agg 10m
  ```

#### Database Management

- **Reset the database:**
  ```bash
  npm start -- reset
  ```
  ⚠️ Warning: This deletes all users and related data.

## Example Workflow

```bash
# Register a new user
npm start -- register alice

# Add some RSS feeds
npm start -- addfeed "Tech Blog" https://example.com/rss
npm start -- addfeed "Dev News" https://dev.example.com/feed.xml

# Start aggregating posts every 30 minutes
npm start -- agg 30m

# In another terminal, browse the latest 5 posts
npm start -- browse 5
```

## Development

- **Generate new migrations:**
  ```bash
  npm run generate
  ```

- **Run migrations:**
  ```bash
  npm run migrate
  ```

## Architecture

The project uses:
- [Drizzle ORM](https://orm.drizzle.team/) for database operations (see [src/lib/db/schema.ts](src/lib/db/schema.ts))
- TypeScript for type safety
- PostgreSQL for data storage
- [fast-xml-parser](https://github.com/NaturalIntelligence/fast-xml-parser) for RSS feed parsing (see [src/lib/rss.ts](src/lib/rss.ts))

## Database Schema

The application uses four main tables:
- **users**: User accounts
- **feeds**: RSS feed sources
- **feed_follows**: Many-to-many relationship between users and feeds
- **posts**: Aggregated blog posts from feeds

See [src/lib/db/schema.ts](src/lib/db/schema.ts) for the complete schema definition.