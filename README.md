# Data Access - MySQL Album Database

A Go application that demonstrates database access patterns using MySQL with the `database/sql` package and the MySQL driver.

## Overview

This project showcases how to:
- Connect to a MySQL database from Go
- Execute SQL queries (SELECT, INSERT, etc.)
- Scan query results into Go structs
- Handle database errors gracefully
- Use parameterized queries to prevent SQL injection

## Project Structure

```
.
├── main.go              # Main application with Album queries and operations
├── create-tables.sql    # SQL schema and sample data
├── go.mod               # Go module definition
└── README.md            # This file
```

## Database Schema

The project uses a single `album` table with the following structure:

| Column | Type | Description |
|--------|------|-------------|
| id | INT (AUTO_INCREMENT) | Primary key |
| title | VARCHAR(128) | Album title |
| artist | VARCHAR(255) | Artist name |
| price | DECIMAL(5,2) | Album price |

Sample data includes albums from John Coltrane, Gerry Mulligan, and Sarah Vaughan.

## Setup

### Prerequisites

- Go 1.25.4 or higher
- MySQL 5.7+ server running locally on port 3306
- MySQL client tools (optional, for manual SQL execution)

### Installation

1. Clone or navigate to the project directory:
   ```bash
   cd /home/mani/projects/data-access
   ```

2. Download dependencies:
   ```bash
   go get github.com/go-sql-driver/mysql@latest
   go mod tidy
   ```

3. Set up the MySQL database:
   ```bash
   mysql -u root -p < create-tables.sql
   ```

4. Configure environment variables:
   ```bash
   export DBUSER=root
   export DBPASS=your_password
   ```

## Usage

### Running the Application

```bash
go run main.go
```

### Available Functions

- **`albumsByArtist(name string)`** - Query all albums by a specific artist
- **`albumByID(id int64)`** - Get a single album by its ID
- **`addAlbum(alb Album)`** - Insert a new album into the database

### Example Code

```go
// Query albums by artist
albums, err := albumsByArtist("John Coltrane")
if err != nil {
    log.Fatal(err)
}
for _, alb := range albums {
    fmt.Printf("%v\n", alb)
}

// Get album by ID
alb, err := albumByID(1)
if err != nil {
    log.Fatal(err)
}
fmt.Printf("%v\n", alb)

// Add new album
newAlbum := Album{Title: "Kind of Blue", Artist: "Miles Davis", Price: 59.99}
alb, err := addAlbum(newAlbum)
if err != nil {
    log.Fatal(err)
}
```

## Dependencies

- **github.com/go-sql-driver/mysql** - MySQL driver for Go's `database/sql` package

## Building

To build an executable:

```bash
go build -o data-access
./data-access
```

## Environment Variables

| Variable | Description | Example |
|----------|-------------|---------|
| DBUSER | MySQL username | `root` |
| DBPASS | MySQL password | `mypassword` |

The application defaults to:
- Host: `127.0.0.1`
- Port: `3306`
- Database: `recordings`

## Error Handling

The application uses error wrapping with `fmt.Errorf` to provide context-aware error messages. Errors from database operations include:
- Function name where the error occurred
- Query parameters (where applicable)
- The underlying error message

## Notes

- All SQL queries use parameterized statements (`?` placeholders) to prevent SQL injection
- The `Album` struct fields must match the database column names and types
- Database connection is stored in a package-level variable `db`
- Remember to set required environment variables before running

## Future Enhancements

- Add transaction support for multi-statement operations
- Implement connection pooling configuration
- Add logging middleware for query monitoring
- Create additional CRUD operations (Update, Delete)
- Add unit tests with mocked database
