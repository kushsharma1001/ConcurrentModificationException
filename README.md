ConcurrentModificationException in List and HashMap iteration

if we try to modifiy the size of list or hashmap while iterating over it, it will throw ConcurrentModificationException.
https://www.journaldev.com/378/java-util-concurrentmodificationexception

Hence, always use ConcurrentHashMap instead of HashMap and CopyOnWriteArrayList instead of ArrayList classes.

Example:
		Map <String, Integer> map = new HashMap();
		map.put("One", 1);
		map.put("Two", 2);
		map.put("Three", 3);
		
		Iterator <String> iter  = map.keySet().iterator();
		
		while(iter.hasNext()){
		  System.out.println(iter.next()); 
		}
    
   The above code works fine bcoz there is no modification. If we try to change the size of the map, it will throw Exception.
   Lets see:
   
Example:
	  	Map <String, Integer> map = new HashMap(); //or   Map <String, Integer> map = Collections.synchroisedMap(new HashMap());  
		map.put("One", 1);
		map.put("Two", 2);
		map.put("Three", 3);
		
		Iterator <String> iter  = map.keySet().iterator();
		
		while(iter.hasNext()){
		  System.out.println(iter.next()); 
      map.put("Four",4);              // trying to modify the map with iterator
		}
    
   The above code will throw ConcurrentModificationException if we use HashMap or even if we use synchronisedMap().
   
   Solution is to use ConcurrentHashMap(); Modification is then allowed with iterator but changes dont reflect and original map is reflected.
   
Example:

    		Map <String, Integer> map = new ConcurrentHashMap();
		map.put("One", 1);
		map.put("Two", 2);
		map.put("Three", 3);
		
		Iterator <String> iter  = map.keySet().iterator();
		
		while(iter.hasNext()){ //iterator.hasNext() is true only till "Three" even if we modified the map in this loop but it doesnt reflect
		  System.out.println(iter.next()); 
		    map.put("Four", 4);
		}
		
		iter  = map.keySet().iterator();
		while(iter.hasNext()){ //this time it runs till "Four"
		  System.out.println(iter.next()); 
		}
    
    Hence, we see that while concurrent modification is allowed in ConcurrentHahMap but it is not reflected in same iterator iteration.
    
    Difference between ConcurrentHashMap and synchronizedHashMap: https://crunchify.com/hashmap-vs-concurrenthashmap-vs-synchronizedmap-how-a-hashmap-can-be-synchronized-in-java/
