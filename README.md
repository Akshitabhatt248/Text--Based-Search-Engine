# Text--Based-Search-Engine
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Scanner;

// Represents a document with an ID and content
class Document {
    String id;
    String content;

    public Document(String id, String content) {
        this.id = id;
        this.content = content;
    }
}

// Main class for the Text-Based Search Engine
class TextSearchEngine {
    private Map<String, List<String>> invertedIndex;

    public TextSearchEngine() {
        invertedIndex = new HashMap<>();
    }

    // Method to add a document to the inverted index
    public void addDocument(Document doc) {
        String[] words = doc.content.toLowerCase().split("\\W+");
        for (String word : words) {
            invertedIndex.computeIfAbsent(word, k -> new ArrayList<>()).add(doc.id);
        }
    }

    // Method to search for documents containing all words in the query
    public List<String> search(String query) {
        String[] queryWords = query.toLowerCase().split("\\W+");
        List<String> result = new ArrayList<>();

        for (String word : queryWords) {
            List<String> docs = invertedIndex.get(word);
            if (docs != null) {
                if (result.isEmpty()) {
                    result.addAll(docs);
                } else {
                    result.retainAll(docs); // Keeps only documents containing all words
                }
            }
        }
        return result;
    }

    // Method to print the inverted index (for debugging or inspection)
    public void printInvertedIndex() {
        System.out.println("Inverted Index:");
        for (Map.Entry<String, List<String>> entry : invertedIndex.entrySet()) {
            System.out.println(entry.getKey() + " -> " + entry.getValue());
        }
    }
}

public class Main {
    public static void main(String[] args) {
        TextSearchEngine searchEngine = new TextSearchEngine();

        // Add documents to the search engine
        searchEngine.addDocument(new Document("1", "Rajiv leaves the sites on 12 oct "));
        searchEngine.addDocument(new Document("2", " tzy is working on game dev "));
        searchEngine.addDocument(new Document("3", "nitish is on exam brake "));
        searchEngine.addDocument(new Document("4", " we did have the project to day and worked on 25 sep "));
        searchEngine.addDocument(new Document("5", "i dont know what i am doing with my life "));

        // Uncomment to print the inverted index for inspection
        // searchEngine.printInvertedIndex();

        // User search
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter search query: ");
        String query = scanner.nextLine();

        List<String> results = searchEngine.search(query);

        // Display results
        if (results.isEmpty()) {
            System.out.println("No documents found for the query: " + query);
        } else {
            System.out.println("Documents found:");
            for (String docId : results) {
                System.out.println("Document ID: " + docId);
            }
        }
        
        scanner.close();
    }
}
