# Guidelines for Workflow in the Golang Boilerplate

## Database Actions on the boilerplate
   ### Model to DB Interactions
- **Model to DB interactions should be receiver functions calling the repository functions.**
  - Example: 
    ```go
    func (u *User) Save() error {
        return db.CreateUser(u)
    }
    ```

   ### Service Layer
- **Your service should call the model functions.**
  - Example: 
    ```go
    func RegisterUser(user *User) error {
        // some codes
        return user.Save()
    }
    ```

  ### Model Scope
- **Any model interacting with the DB should be scoped to that model, and the function should be a receiver.**
  - Example: 
    ```go
    func (u *User) FindByID(id string) (*User, error) {
        return db.GetUserByID(id)
    }
    ```
   #### Ensure anything related to a model interacting with the database is scoped to that model, and the function should be a receiver. This avoids redundancy and keeps the code organized.


## Running a Docker Postgres Server
Use this command if you don't have a database setup locally and want to use postgres:
```sh
docker run --name fin \
  -e POSTGRES_USER=myuser \
  -e POSTGRES_PASSWORD=mypassword \
  -e POSTGRES_DB=mydatabase \
  -e PGDATA=/var/lib/postgresql/data/pgdata \
  -v /my/local/path:/var/lib/postgresql/data \
  -p 127.0.0.1:5432:5432 \
  -d postgres:latest \
  -c ssl=off \    
  -c timezone=Africa/Lagos
```

## Database Model Changes

If you need changes to the database models follow this steps:

### Procedure for Database Model Changes
- **Consult the database team with the specification of what you want if you need changes to the database models.**

### Example

#### Requesting Changes
Suppose you need to add a new field `date_of_birth` to the `users` table.

**Draft a specification request:**
  ```plaintext
    I would like to request the addition of a new field `date_of_birth` to the `users` table. Below are the details:

    - Table: users
    - New Field: date_of_birth
    - Data Type: DATE
    - Description: This field will store the date of birth of the user.
  ```

  
**Requested result:**
  ```go
    type User struct {
        ID           string    `gorm:"type:uuid;primaryKey;unique;not null" json:"id"`
        Name         string    `gorm:"type:varchar(255);not null" json:"name"`
        Email        string    `gorm:"type:varchar(255);unique" json:"email"`
        DateOfBirth  time.Time `gorm:"type:date" json:"date_of_birth"` // New field
        CreatedAt    time.Time `gorm:"column:created_at; not null; autoCreateTime" json:"created_at"`
        UpdatedAt    time.Time `gorm:"column:updated_at; null; autoUpdateTime" json:"updated_at"`
    }
  ```
 **Send the request to the database team for approval and implementation.**


## Git Best Practices

### Cloning the Repository
1. Clone the repository:
   ```sh
   git clone "url you just copied"
   ```
   Example:
   ```sh
   git clone git@github.com:this-is-you/hng_boilerplate.git
   ```
   Replace `this-is-you` with your GitHub username.

### Creating a New Branch
2. Create your new feature/chore/bug branch for working:
   ```sh
   git checkout -b feat/add-auth
   ```
   Or:
   ```sh
   git checkout -b chore/update-auth-feature
   ```
   Or:
   ```sh
   git checkout -b fix/auth-bug-fix
   ```
   Read more on best practices here: [Git Best Practices](https://medium.com/@shinjithkanhangad/git-good-best-practices-for-branch-naming-and-commit-messages-a903b9f08d68).

### Making Changes
3. Make necessary changes, add and commit those changes:
   ```sh
   git add .
   git commit -m "feat: basic auth feature"
   ```
   Refer to the blog post above for best practices with commit naming.

### Pushing Changes
4. Push your changes to the GitHub upstream (your repo):
   ```sh
   git push -u origin your-branch-name
   ```
   Replace `your-branch-name` with the name of the branch you created earlier.

### Submitting Changes for Review
5. Submit your changes for review. Compare your changes against the dev or development branch of our repos. Your PR naming should look similar to the commit name:
   ```sh
   feat: basic authentication
   ```
   Or:
   ```sh
   feat: created Login Page
   ```
   Or:
   ```sh
   chore: updated readme file
   ```

## Guidelines for Making PRs

- **E2E Tests**: If you implemented a new feature, ensure you write end-to-end tests.
- **Running Tests**: Before pushing, ensure all tests are working in the test folder using the command:
  ```sh
  go test ./... -v
  ```
- **Pull Before Pushing**: Always pull from the dev branch before pushing.
- **Push**: Always push to the new branch created
- **Making PR**: Always make PR to dev branch

## Endpoint URLs

- **Production Endpoint**: [https://api-golang.boilerplate.hng.tech](https://api-golang.boilerplate.hng.tech)
- **Staging Endpoint**: [https://staging-api-golang.boilerplate.hng.tech](https://staging-api-golang.boilerplate.hng.tech)
- **Dev Endpoint**: [https://deployment-api-golang.boilerplate.hng.tech](https://deployment-api-golang.boilerplate.hng.tech)

Once you request a PR to dev and a team lead or mentor approves it, test your implementation on the dev endpoint.

**Note**: 
    - If you don't follow any of these guidelines, your **PR will not be approved**.
    - If you have issue with any of these consult the group for help
