# Library-Management-System

import java.util.*;

// Book Class

class Book{

    private String title;
    private String author;
    private String isbn;
    private int publicationYear;
    private boolean isBorrowed;

    public Book(String title, String author, String isbn, int publicationYear){
        this.title = title;
        this.author = author;
        this.isbn = isbn;
        this.publicationYear = publicationYear;
        this.isBorrowed = false;
    }

    public String getTitle(){ 
        return title; 
    }
    
    public String getAuthor(){ 
        return author; 
    }
    
    public String getIsbn(){ 
        return isbn; 
    }
    
    public int getPublicationYear(){ 
        return publicationYear;    
    }
    
    public boolean isBorrowed(){ 
        return isBorrowed; 
    }
    
    public void setBorrowed(boolean borrowed){ 
        this.isBorrowed = borrowed;     
    }

    @Override
    public String toString(){
        return "Book{" +
                "title='" + title + '\'' +
                ", author='" + author + '\'' +
                ", isbn='" + isbn + '\'' +
                ", publicationYear=" + publicationYear +
                ", isBorrowed=" + isBorrowed +
                '}';
    }
}

// Patron Class
class Patron{

    private String name;
    private String id;
    private List<Book> borrowedBooks;

    public Patron(String name, String id){
        this.name = name;
        this.id = id;
        this.borrowedBooks = new ArrayList<>();
    }

    public String getName(){ 
        return name; 
    }
    
    public String getId(){ 
        return id; 
    }
    
    public List<Book> getBorrowedBooks(){ 
        return borrowedBooks; 
    }

    public void borrowBook(Book book){
        borrowedBooks.add(book);
    }

    public void returnBook(Book book){
        borrowedBooks.remove(book);
    }

    @Override
    public String toString(){
        return "Patron{" +
                "name='" + name + '\'' +
                ", id='" + id + '\'' +
                ", borrowedBooks=" + borrowedBooks +
                '}';
    }
}

// Library Class
class Library{
    private Map<String, Book> books;
    private Map<String, Patron> patrons;

    public Library(){
        books = new HashMap<>();
        patrons = new HashMap<>();
    }

    public void addBook(Book book){
        books.put(book.getIsbn(), book);
    }

    public void removeBook(String isbn){
        books.remove(isbn);
    }

    public void updateBook(String isbn, Book updatedBook){
        books.put(isbn, updatedBook);
    }

    public Book searchBookByTitle(String title){
        return books.values().stream().filter(book -> book.getTitle().equalsIgnoreCase(title)).findFirst().orElse(null);
    }

    public Book searchBookByAuthor(String author){
        return books.values().stream().filter(book -> book.getAuthor().equalsIgnoreCase(author)).findFirst().orElse(null);
    }

    public Book searchBookByIsbn(String isbn){
        return books.get(isbn);
    }

    public void addPatron(Patron patron){
        patrons.put(patron.getId(), patron);
    }

    public void updatePatron(String id, Patron updatedPatron){
        patrons.put(id, updatedPatron);
    }

    public void checkoutBook(String isbn, String patronId){
        Book book = books.get(isbn);
        Patron patron = patrons.get(patronId);

        if (book != null && patron != null && !book.isBorrowed()){
            book.setBorrowed(true);
            patron.borrowBook(book);
            System.out.println("Book checked out successfully.");
        } else{
            System.out.println("Checkout failed: Book may already be borrowed or patron not found.");
        }
    }

    public void returnBook(String isbn, String patronId){
        Book book = books.get(isbn);
        Patron patron = patrons.get(patronId);

        if (book != null && patron != null && book.isBorrowed()){
            book.setBorrowed(false);
            patron.returnBook(book);
            System.out.println("Book returned successfully.");
        } else{
            System.out.println("Return failed: Book may not be borrowed or patron not found.");
        }
    }

    public void displayInventory(){
        books.values().forEach(System.out::println);
    }
}

// Main Class
public class LibraryManagementSystem{
    public static void main(String[] args){
        Library library = new Library();

        Book book1 = new Book("Java Programming", "John Doe", "123456", 2020);
        Book book2 = new Book("Effective Java", "Joshua Bloch", "789101", 2018);
        library.addBook(book1);
        library.addBook(book2);

        Patron patron1 = new Patron("Alice", "P001");
        library.addPatron(patron1);

        library.checkoutBook("123456", "P001");
        library.displayInventory();
        library.returnBook("123456", "P001");
        library.displayInventory();
    }
}
