### EX5 Information Retrieval Using Boolean Model in Python
### DATE: 18/08/2026
### NAME: MANJUSRI KAVYA R
### REGISTER NUMBER: 212224040186
### AIM: To implement Information Retrieval Using Boolean Model in Python.
### Description:
<div align = "justify">
The Boolean model in Information Retrieval (IR) is a fundamental model used for searching and retrieving information from a collection of documents. It operates on the principles of set theory and logic, where documents are represented as sets of terms or words, and queries are expressed as Boolean expressions using logical operators such as AND, OR, and NOT.
  
### Procedure:
1. ***Initialize the BooleanRetrieval class:*** The BooleanRetrieval class is defined to manage the indexing and searching of documents.
2. ***Constructor and Index Initialization:*** The class constructor initializes an empty index to store the inverted index mapping terms to documents.
3. ***Indexing Documents:***
    <p> a) The index_document method is responsible for indexing documents.
    <p> b) Tokenize the text content of documents, converting them into lowercase terms.
    <p> c) For each term in the document, it adds an entry in the index, associating the term with the document ID. </p>
4. ***Fetch Web Page Text:***
    <p>a) The fetch_webpage_text method uses the requests library to fetch content from a given URL.
    <p>b) Extract text content from the fetched HTML using BeautifulSoup.
    <p>c) The extracted text is returned for further processing.
5. ***Boolean Search:***
    <p>a) The boolean_search method performs Boolean searches on the indexed documents.
    <p>b) Tokenize the input query and iterates through its terms.
    <p>c) For each term in the query, it retrieves documents containing that term and performs Boolean operations (AND, OR, NOT) based on the query's structure.

### Program:
```
import numpy as np
import pandas as pd

class BooleanRetrieval:

    def __init__(self):
        self.index = {}
        self.documents_matrix = None

    def index_document(self, doc_id, text):
        terms = text.lower().split()
        print("Document -", doc_id, terms)

        for term in terms:
            if term not in self.index:
                self.index[term] = set()
            self.index[term].add(doc_id)

    def create_documents_matrix(self, documents):
        terms = list(self.index.keys())
        num_docs = len(documents)
        num_terms = len(terms)

        self.documents_matrix = np.zeros(
            (num_docs, num_terms), dtype=int
        )

        for i, (doc_id, text) in enumerate(documents.items()):
            doc_terms = text.lower().split()

            for term in doc_terms:
                if term in self.index:
                    term_id = terms.index(term)
                    self.documents_matrix[i, term_id] = 1

    def print_documents_matrix_table(self):
        df = pd.DataFrame(
            self.documents_matrix,
            columns=self.index.keys()
        )
        print("\nDocument-Term Matrix:")
        print(df)

    def print_all_terms(self):
        print("\nAll terms in the documents:")
        print(list(self.index.keys()))

    def boolean_search(self, query):
        query = query.lower().split()

        all_documents = set()

        for docs in self.index.values():
            all_documents.update(docs)

        if len(query) == 1:
            return self.index.get(query[0], set())

        result = None
        operation = "AND"
        i = 0

        while i < len(query):

            word = query[i]

            if word == "and":
                operation = "AND"

            elif word == "or":
                operation = "OR"

            elif word == "not":
                operation = "NOT"

            else:
                current_docs = self.index.get(word, set())

                if result is None:
                    if operation == "NOT":
                        result = all_documents - current_docs
                    else:
                        result = current_docs.copy()

                else:
                    if operation == "AND":
                        result = result & current_docs

                    elif operation == "OR":
                        result = result | current_docs

                    elif operation == "NOT":
                        result = result - current_docs

                operation = "AND"

            i += 1

        return result


if __name__ == "__main__":

    indexer = BooleanRetrieval()

    documents = {
        1: "Python is a programming language",
        2: "Information retrieval deals with finding information",
        3: "Boolean models are used in information retrieval"
    }

    # Index documents
    for doc_id, text in documents.items():
        indexer.index_document(doc_id, text)

    # Create document-term matrix
    indexer.create_documents_matrix(documents)

    # Display matrix
    indexer.print_documents_matrix_table()

    # Display all terms
    indexer.print_all_terms()

    # Get query from user
    query = input("\nEnter your boolean query: ")

    # Search
    results = indexer.boolean_search(query)

    if results:
        print(f"Results for '{query}': {sorted(results)}")
    else:
        print("No results found for the query.")
```

### Output:

<img width="901" height="297" alt="image" src="https://github.com/user-attachments/assets/bd6716aa-c135-4145-b68d-f2973385491d" />

<img width="888" height="297" alt="image" src="https://github.com/user-attachments/assets/b50bf7bc-5768-4e87-ac3b-221f55f1e4d6" />

<img width="890" height="295" alt="image" src="https://github.com/user-attachments/assets/8bb18f92-86ab-4919-b949-3e2d160a1bc7" />

### Result:

Implementation of Information Retrieval Using Boolean Model in Python is successfully completed.
