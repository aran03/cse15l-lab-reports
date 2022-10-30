
```

import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;;

class Handler implements URLHandler {
    // The one bit of state on the server: a number that will be manipulated by
    // various requests.
    int num = 0;
    ArrayList<String> searchTerms = new ArrayList<String>();

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return String.format("Output:");
        } 
        else if (url.getPath().contains("/add")) {
            String[] parameters = url.getQuery().split("=");
            if (parameters[0].equals("s")) {
                searchTerms.add(parameters[1]);
                return String.format("thingy added!");
            }         
        }
        else if (url.getPath().contains("/search")) {
            String[] parameters = url.getQuery().split("=");
            String appendedSearch = ";";
            if (parameters[0].equals("s")){
                for(int i = 0; i < searchTerms.size(); i++)   if(searchTerms.get(i).contains(parameters[1])) appendedSearch = appendedSearch.concat(searchTerms.get(i) + " ");
                return String.format("Search Results: " + parameters[1] + ": " + appendedSearch);
            }
        }
        else {
            System.out.println("Path: " + url.getPath());
            String all = "";
            if (url.getPath().contains("/allEntries")) {
                for(int i = 0; i < searchTerms.size(); i++){
                    all.concat(searchTerms.get(i));
                }
            }
            return String.format("All Entries: ");
        }
        return String.format("404 Not Found!");
    }
}

public class SearchEngine {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}


```
![Image](sslab2-1.png)
###### First Screenshot Explanation:
In this screenshot, I added the word never. As expected, the search engine displays that it was added.
When adding a term like just now, this logic within the handleRequest() method is triggered: 

'''
        else if (url.getPath().contains("/add")) {
            String[] parameters = url.getQuery().split("=");
            if (parameters[0].equals("s")) {
                searchTerms.add(parameters[1]);
                return String.format("thingy added!");
            }         
        }
'''

This logic basically just says that when the url path encounters the /add argument, it will add the following characters to an arrayList I have called searchTerms.

![Image](sslab2-2.png)
###### Second Screenshot Explanation:
In this screenshot, I added the word nemo. As expected, the search engine displays that it was added.
When adding a term like just now, this logic within the handleRequest() method is triggered: 

'''
        else if (url.getPath().contains("/add")) {
            String[] parameters = url.getQuery().split("=");
            if (parameters[0].equals("s")) {
                searchTerms.add(parameters[1]);
                return String.format("thingy added!");
            }         
        }
'''

This logic basically just says that when the url path encounters the /add argument, it will add the following characters to an arrayList I have called searchTerms.

![Image](sslab2-3.png)
###### Third Screenshot Explanation:
In this screenshot, I searched for the term "ne". As expected, the search engine displays nemo and never, because both words were added and contain "ne".
When seraching for a term like just now, this logic within the handleRequest() method is triggered: 

'''
        else if (url.getPath().contains("/search")) {
            String[] parameters = url.getQuery().split("=");
            String appendedSearch = ";";
            if (parameters[0].equals("s")){
                for(int i = 0; i < searchTerms.size(); i++)   if(searchTerms.get(i).contains(parameters[1])) appendedSearch = appendedSearch.concat(searchTerms.get(i) + " ");
                return String.format("Search Results: " + parameters[1] + ": " + appendedSearch);
            }
        }
'''

This logic basically just says that when the url path encounters the /search argument, it will determine all characters after "?s=" to be the search term and will check all the items in the ArrayList searchTerms to see which Strings contain the search term. In this case, the characters after "?s=" are "ne", and the two items in the ArrayList searchTerms are checked to see if they contain "ne", which they do. 