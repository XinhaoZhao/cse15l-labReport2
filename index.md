# Part 1  
Code for search engine:  
```
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;
import java.util.List;

class Handler implements URLHandler {
  ArrayList<String> list = new ArrayList<>();

  public String handleRequest(URI url) {
      if (url.getPath().equals("/")) {
          return String.format("Current string: ", printAll(list));
      } else if (url.getPath().contains("/add")) {
        String[] parameters = url.getQuery().split("=");
        list.add(parameters[1]);
          return String.format("String updated, new string is: " + printAll(list));
      } else if (url.getPath().contains("/search")){
        String[] parameters = url.getQuery().split("=");
        return String.format("Results are: " + search(list, parameters[1]));
      }
      else {
          return "404 Not Found!";
      }
  }

  public String search(ArrayList<String> arr, String query){
    String result = "";
    for(String s : arr){
        if (s.contains(query)){
            result += " ";
            result += s;
        }
    }
    return result;
  }

  public String printAll(ArrayList<String> arr){
    String result = "";
    for (String s : arr){
        if(result.length()==0){result += s;}
        else{
            result += ", ";
            result += s;
        }
    }
    return result;
  }
}


class SearchEngine {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing any input, please input a string.");
        }

        int port = Integer.parseInt(args[0]);
        Server.start(port, new Handler());
    }
    
  }
```  
Screenshots of running:  
1. ![image](https://i.imgur.com/vQNNcJB.png)  
In the above screenshot, the programm called handleRequest(). The argument is "xinhao" which is stored in list. The value of list changes,  
now the value of list is {"xinhao"}.  
  
2. ![image](https://i.imgur.com/dtkh1jQ.png)  
In the above screenshot, the programm called handleRequest(). The argument is "zhao" which is stored in list. The value of list changes,  
now the value of list is {"xinhao", "zhao"}.    
  
3. ![image](https://i.imgur.com/Ioe6fjj.png)  
In the above screenshot, the programm called handleRequest() and search(). The argument is "in". The value result is "xinhao" because xinhao contains "in".  
  
# Part 2
1. In the ListExample file, the failure-inducing input for function filter() is:  
```
public class ListTests {
    String[] arr = new String[] {"a", "b", "c", "d"};
    List<String> eg = Arrays.asList(arr);
    StringChecker sc = new StringChecker() {
        public boolean checkString(String s){
            if(s.length()==1){return true;}
            return false;
        } 
    };
    
    @Test
    public void testFilter(){

        assertEquals(eg, ListExamples.filter(eg, sc));
    }
```  
The symptom is that the expected output is `[a, b, c, d]` while the actual output is `[d, c, b, a]`.  
The bug is that the program always puts the checked element of the list at index 0, which is the beginning of the list. 
By doing so, it changes the order of the original input. Therefore, we see that the actual output is not in the same order as the
original input.  
  
2. In the ArrayExamples file, the failure-inducing input for reverseInPlace function is  
```
@Test 
	public void testReverseInPlace() {
    int[] input1 = { 3,2,1 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{1,2,3}, input1);
	}
```  
The bug is that in each iteration, one element inside the array is being replaced by another one. In this case,  
3 is assigned to be 1, while 1 stays unchanged. Now, the array is [1,2,1], in which we see that 3 is completely replaced,  
therefore the incorrect output.  




