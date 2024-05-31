# lab-ten code optimization practice


### Objective:
Enhance the performance of provided code snippets by applying code-level optimization techniques manually. This lab focuses on deepening your understanding and application of optimization principles in coding.


### Instructions:

At the beginning of the lab, you will receive a set of code snippets that contain common performance issues. These snippets will come from different programming languages, including JavaScript, Java, and C#, to cover a variety of scenarios and common inefficiencies found in real-world programming.

### Provided Code Snippets:

##### JavaScript Snippet:

	// Inefficient loop handling and excessive DOM manipulation
	function updateList(items) {
	  let list = document.getElementById("itemList");
	  list.innerHTML = "";
	  for (let i = 0; i < items.length; i++) {
	    let listItem = document.createElement("li");
	    listItem.innerHTML = items[i];
	    list.appendChild(listItem);
	  }
	}

#### Solution

Code analysis and Findings

* Algorithm is creating nested element in each bucle iteration
* it could be refreshing View many times and cause bad experience in UI

Optimization

* We could pre initialize a not existent container/fragment in memory, and we're going to add nested items into it 
* When iterations are finished we could add FragmentView into general DOM,
* Using that solution ensuring we interact and refreshing view only 1 time 
* Avoid bad experience in UI for clients

Pseudocode

    function updateList(items) {
      
        let list = document.getElementById("itemList");
    
        const fragment = document.createDocumentFragment();

        items.forEach((_, i) => {
            const listItem = document.createElement("li");
            listItem.innerHTML =  items[i];

            fragment.appendChild(listItem);
        });
    
        // interact only 1 time with UI
        document.body.appendChild(fragment);
    }

`------------------------------`

#### Java Snippet:

	// Redundant database queries
	public class ProductLoader {
	    public List<Product> loadProducts() {
	        List<Product> products = new ArrayList<>();
	        for (int id = 1; id <= 100; id++) {
	            products.add(database.getProductById(id));
	        }
	        return products;
	    }
	}

#### Solution

Code analysis and Findings

* Multiple and several queries to database by Id
* On each iteration call to database and store in array structure


Optimization

* Redefine method, to use pagination and receive params to indicate start index and number of elements
* Create sub methods Single responsibility to getData 
* Use Select query specifying the specific and necessary fields that we will use
* Use while to fill the elements in ArrayList

* We eliminate unnecessary queries to database, we get all elements in 1 query
* We optimize, fragment and cleaning algorithm using Single responsibility Principle

Pseudocode

	public class ProductLoader {
	    
	    public List<Product> loadProducts(int startPage, int elements) {
		
			// call method to  retrieve data
			List<Product> = retrieveData(startPage, elements);

			log.info("Products retrieved: {}", products.size())

		    return products;
		}
        
		// Method Single responsibility to retrieve Data
		private List<Product> retrieveData(int startPage, int elements){

			 return entityManager.createQuery("SELECT p.id, p.desc, p.price FROM Product p where p.id >= :start and p.id <= :end")
                    .setParameter("start", startPage)
                    .setParameter("end", elements)
                    .getResultList();
		}

	}

`------------------------------`

#### C# Snippet:

	// Unnecessary computations in data processing
	public List<int> ProcessData(List<int> data) {
	    List<int> result = new List<int>();
	    foreach (var d in data) {
	        if (d % 2 == 0) {
	            result.Add(d * 2);
	        } else {
	            result.Add(d * 3);
	        }
	    }
	    return result;
	}

#### Solution

Code analysis and Findings

* There are two validations and operations in sequence , this could be slow process

Optimization

* We could use parallel for each to process data fast and efficiently, anyway the results are storing in the same array 

Pseudocode

    public List<int> ProcessData(List<int> data) {

            var result = new ConcurrentBag<int>();

            Parallel.ForEach(numbers, number => 
            {
                 if (d % 2 == 0) {
	                result.Add(d * 2);
                } else {
                    result.Add(d * 3);
                }
            });

            return numbers.ToList();
    }