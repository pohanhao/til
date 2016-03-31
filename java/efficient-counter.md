# Efficient Counter

First of all, We need to define a mutable class name **MutableInteger** with lombok annotation

	@AllArgsConstructor
	class MutableInteger {
		@Getter	
		@Setter	
		private int val;
	}

Secondly, the counter is improved and changed to the following:

	HashMap<String, MutableInteger> efficientCounter = new HashMap<String, MutableInteger>();
 
	for (String word : words) {
		MutableInteger initValue = new MutableInteger(1);
		MutableInteger oldValue = efficientCounter.put(word, initValue);
 
		if(oldValue != null){
			initValue.set(oldValue.get() + 1);
		}
	}

----------

But in **java 8**, you can write a counter simply.

	String[] array = {"program", "creek", "program", "creek", "java", "web", "program"};
	Stream<String> stream = Stream.of(array).parallel();
	Map<String, Long> counter = stream.collect(Collectors.groupingBy(String::toString, Collectors.counting()));


**Reference**

1. http://www.programcreek.com/2013/10/efficient-counter-in-java/
2. http://www.programcreek.com/2014/01/java-8-counter/