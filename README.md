# Exp-04-Spring-Boot-with-REST-API-and-Hibernate-Integration

## AIM:
To develop a Spring Boot application to store and retrieve data from a Movies database using Object Relational Mapping (ORM) with Hibernate and expose it via REST APIs.

## ALGORITHM:
Create Spring Boot project with dependencies:

Spring Web

Spring Data JPA

H2 or MySQL Database

Configure application.properties with DB connection and JPA settings.

Create Movie entity with fields like id, title, genre, rating, and year.

Create MovieRepository interface extending JpaRepository.

Create MovieController to define REST endpoints for CRUD operations:

GET /movies

GET /movies/{id}

POST /movies

PUT /movies/{id}

DELETE /movies/{id}


## PROGRAM CODE (Main Files):
### application.properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
### Movie.java

@Entity
public class Movie {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    private String genre;
    private int year;
    private double rating;

    // Getters and Setters
}
### MovieRepository.java
java
Copy
Edit
public interface MovieRepository extends JpaRepository<Movie, Long> {}
### MovieController.java
@RestController
@RequestMapping("/movies")
public class MovieController {
    @Autowired
    private MovieRepository repo;

    @GetMapping
    public List<Movie> getAllMovies() {
        return repo.findAll();
    }

    @GetMapping("/{id}")
    public ResponseEntity<Movie> getMovieById(@PathVariable Long id) {
        return repo.findById(id)
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }

    @PostMapping
    public Movie addMovie(@RequestBody Movie movie) {
        return repo.save(movie);
    }

    @PutMapping("/{id}")
    public ResponseEntity<Movie> updateMovie(@PathVariable Long id, @RequestBody Movie movieDetails) {
        return repo.findById(id).map(movie -> {
            movie.setTitle(movieDetails.getTitle());
            movie.setGenre(movieDetails.getGenre());
            movie.setYear(movieDetails.getYear());
            movie.setRating(movieDetails.getRating());
            return ResponseEntity.ok(repo.save(movie));
        }).orElse(ResponseEntity.notFound().build());
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteMovie(@PathVariable Long id) {
        return repo.findById(id).map(movie -> {
            repo.delete(movie);
            return ResponseEntity.ok().build();
        }).orElse(ResponseEntity.notFound().build());
    }
}

## OUT PUT:
<img width="946" height="231" alt="{8EB7351B-3000-4B66-8A1B-15A9BCD996FF}" src="https://github.com/user-attachments/assets/b355f7c3-fcfc-451d-a141-c950f450927c" />
<img width="942" height="171" alt="{E6577C36-7FC6-4708-8136-197093558C74}" src="https://github.com/user-attachments/assets/6aa19799-3720-4907-99a2-cc82a9bbca14" />
<img width="952" height="162" alt="{8DF90D97-C731-4572-B67A-D70DD4E3A71C}" src="https://github.com/user-attachments/assets/2a81c225-ff1c-4af8-8515-06037b60e3c1" />

## RESULT: Thus,the Spring Boot application to store and retrieve data from a Movies database using Object Relational Mapping (ORM) with Hibernate and expose it via REST APIs implemented and executed successfully.



