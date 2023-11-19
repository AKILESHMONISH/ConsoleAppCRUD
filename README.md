import java.io.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

class Movie {
    private String name;
    private String director;
    private int releaseYear;
    private String language;
    private double rating;

    public Movie(String name, String director, int releaseYear, String language, double rating) {
        this.name = name;
        this.director = director;
        this.releaseYear = releaseYear;
        this.language = language;
        this.rating = rating;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDirector() {
        return director;
    }

    public void setDirector(String director) {
        this.director = director;
    }

    public int getReleaseYear() {
        return releaseYear;
    }

    public void setReleaseYear(int releaseYear) {
        this.releaseYear = releaseYear;
    }

    public String getLanguage() {
        return language;
    }

    public void setLanguage(String language) {
        this.language = language;
    }

    public double getRating() {
        return rating;
    }

    public void setRating(double rating) {
        this.rating = rating;
    }

    @Override
    public String toString() {
        return "Movie{" +
                "name='" + name + '\'' +
                ", director='" + director + '\'' +
                ", releaseYear=" + releaseYear +
                ", language='" + language + '\'' +
                ", rating=" + rating +
                '}';
    }
}

class MovieManager {
    private static final String FILE_PATH = "movies.txt";
    private List<Movie> movies;

    public MovieManager() {
        this.movies = new ArrayList<>();
        loadMovies();
    }

    private void loadMovies() {
        try (BufferedReader reader = new BufferedReader(new FileReader(FILE_PATH))) {
            String line;
            while ((line = reader.readLine()) != null) {
                String[] parts = line.split(",");
                String name = parts[0].trim();
                String director = parts[1].trim();
                int releaseYear = Integer.parseInt(parts[2].trim());
                String language = parts[3].trim();
                double rating = Double.parseDouble(parts[4].trim());
                Movie movie = new Movie(name, director, releaseYear, language, rating);
                movies.add(movie);
            }
        } catch (IOException e) { }
    }

    public void showAllMovies() {
        System.out.println("\nAll Movies:");
        movies.forEach(System.out::println);
    }


   public void addMovie(String name, String director, int releaseYear, String language, double rating) {
        Movie movie = new Movie(name, director, releaseYear, language, rating);
        movies.add(movie);
        System.out.println("Movie added successfully!");
    }

    public void updateMovie(String name, String director, int releaseYear, String language, double rating) {
        for (Movie movie : movies) {
            if (movie.getName().equalsIgnoreCase(name)) {
                movie.setDirector(director);
                movie.setReleaseYear(releaseYear);
                movie.setLanguage(language);
                movie.setRating(rating);
                System.out.println("Movie details updated successfully!");
                return;
            }
        }
        System.out.println("Movie not found.");
    }

    public void deleteMovie(String name) {
        for (Movie movie : movies) {
            if (movie.getName().equalsIgnoreCase(name)) {
                movies.remove(movie);
                System.out.println("Movie deleted successfully!");
                return;
            }
        }
        System.out.println("Movie not found.");
    }

    public void filterMoviesByDirector(String director) {
        System.out.println("\nMovies directed by " + director + ":");
        boolean found = false;
        for (Movie movie : movies) {
            if (movie.getDirector().equalsIgnoreCase(director)) {
                System.out.println(movie);
                found = true;
            }
        }
        if (!found) {
            System.out.println("No movies found for this director.");
        }
    }

    public void searchForMovieByName(String name) {
        for (Movie movie : movies) {
            if (movie.getName().equalsIgnoreCase(name)) {
                System.out.println("\nMovie Found:");
                System.out.println(movie);
                return;
            }
        }
        System.out.println("Movie not found.");
    }

    public void saveMovies() {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(FILE_PATH))) {
            for (Movie movie : movies) {
                writer.write(String.format("%s, %s, %d, %s, %.1f%n",
                        movie.getName(), movie.getDirector(), movie.getReleaseYear(), movie.getLanguage(), movie.getRating()));
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

public class MovieApp {
    public static void main(String[] args) {
        MovieManager movieManager = new MovieManager();

        while (true) {
            displayMenu();
            int choice = getUserChoice();

            switch (choice) {
                case 1:
                    movieManager.showAllMovies();
                    break;
                case 2:
                    addMovie(movieManager);
                    break;
                case 3:
                    System.out.print("Enter director's name: ");
                    String director = new Scanner(System.in).nextLine();
                    movieManager.filterMoviesByDirector(director);
                    break;
                case 4:
                    searchForMovie(movieManager);
                    break;
                case 5:
                    updateMovie(movieManager);
                    break;
                case 6:
                    deleteMovie(movieManager);
                    break;
                case 7:
                    movieManager.saveMovies();
                    System.out.println("Goodbye!");
                    System.exit(0);
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }

    private static void displayMenu() {
        System.out.println("\nOptions:");
        System.out.println("1. Show all Movies");
        System.out.println("2. Add a New Movie");
        System.out.println("3. Filter Movies by Director");
        System.out.println("4. Search for a Movie");
        System.out.println("5. Update a Movie's Details");
        System.out.println("6. Delete a Movie");
        System.out.println("7. Quit");
    }

    private static int getUserChoice() {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter your choice: ");
        return scanner.nextInt();
    }

    private static void addMovie(MovieManager movieManager) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("\nEnter Movie Details:");
        System.out.print("Name: ");
        String name = scanner.nextLine();
        System.out.print("Director: ");
        String director = scanner.nextLine();
        System.out.print("Release Year: ");
        int releaseYear = scanner.nextInt();
        System.out.print("Language: ");
        scanner.nextLine();
        String language = scanner.nextLine();
        System.out.print("Rating: ");
        double rating = scanner.nextDouble();

        movieManager.addMovie(name, director, releaseYear, language, rating);
    }

    private static void searchForMovie(MovieManager movieManager) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("\nEnter Movie Name to Search: ");
        String searchName = scanner.nextLine();
        movieManager.searchForMovieByName(searchName);
    }

    private static void updateMovie(MovieManager movieManager) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("\nEnter Movie Name to Update: ");
        String updateName = scanner.nextLine();
        System.out.print("Enter Updated Director: ");
        String director = scanner.nextLine();
        System.out.print("Enter Updated Release Year: ");
        int releaseYear = scanner.nextInt();
        scanner.nextLine();
        System.out.print("Enter Updated Language: ");
        String language = scanner.nextLine();
        System.out.print("Enter Updated Rating: ");
        double rating = scanner.nextDouble();

        movieManager.updateMovie(updateName, director, releaseYear, language, rating);
    }

    private static void deleteMovie(MovieManager movieManager) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("\nEnter Movie Name to Delete: ");
        String deleteName = scanner.nextLine();
        movieManager.deleteMovie(deleteName);
    }
}
