# C-Collections
A small library of C "template" data structures.

[Array.h](#arrayh)

[LinkedList.h](#linkedlisth)

[DoubleLinkedList.h](#doublelinkedlisth)

[Stack.h](#stackh)

[PriorityQueue.h](#priorityqueueh)

[HashMap.h](#hashmaph)

[HashSet.h](#hashseth)

[String.h](#stringh)

[string_utils.h](#string_utilsh)

[memory.h](#memoryh)

[print.h](#printh)

[Hash.h](#hashh)

[TODO](#todo)

So this is really just a library of common data structures that i otherwise find myself rewriting all the time.
Most of the data structures are implemented with a *kind of* template-engine using macros to generate code for a specific type.
The containers also have foreach loops, they too implemented with macros.
Everything in here covers only basic functionality.

----

# Array.h
A dynamic array. As with any array random indexing is fast, adding elements will usually be O(1), but not when it needs to expand the underlying array.

    Exposes functions:
    
        - init_array(Type)
            Macro to generate code for an array containing elements of type Type. E.g init_array(int) will generate the type Array_int along with functions for Array_int,
            like array_int_new(), and array_int_push()
            
        - array_Type_new()
            Returns empty Array of Type with initial capacity of 16 elements
            
        - array_Type_hash(const Array_Type*)
            Returns 32 bit hash value generated from running murmurhash on the underlying array of elements
            
        - array_Type_free(Array_Type*)
            Frees underlying array, and sets everything to 0
            
        - array_Type_copy(const Array_Type*)
            Returns deep copy of the Array passed as argument
            
        - array_Type_expand(Array_Type*)
            Doubles the capacity of the array. Really shouldn't be used since the array handles this automatically, but it's there...
            
        - array_Type_push(Array_Type*, const Type)
            Pushes item to the back of the Array, expands if neccessary
            
        - array_foreach(Type, array, lambda)
            Macro that loops through all elements in array and calls lambda, passing them as argument
            lambda should be a function or macro taking a single parameter of type Type.
            array should be the Array_Type to iterate over.
            Type is not actually used, but included for consistency with the other data types
            
----

# LinkedList.h
A linked list, first in first out. Pushing and popping elements will always be O(1), does not implement random indexing.

    Exposes functions:
        
        - init_list(Type)
            Macro to generate code for a List for type Type. E.g init_list(int) will generate code for List_int along with functions for List_int such as list_int_new(), 
            and list_int_push()
            
        - list_Type_new()
            Returns an empty List of Type
            
        - list_Type_copy(const List_Type*)
            Returns a deep copy of the List passed as argument
           
        - list_Type_push(List_Type*, const Type)
            Pushes the passed item to the back of the list
            
        - list_Type_pop(List_Type*, Type*)
            Will copy the first item in the List into the pointer and remove it from the List, returns true if an item was successfully popped
            
        - list_Type_peek(const List_Type, Type*)
            Will copy the first item in the List into the pointer
            
        - list_Type_to_array(const List_Type*)
            Will return a newly allocated array containing all elements in the List in order. Returns a NULL pointer if List is empty
            
        - list_Type_hash(const List_Type*)
            Returns 32 bit hash generated by xor:ing together the individual murmur-hashes of all items in the List
            
        - list_Type_free(List_Type*)
            Frees any resources used by the List, and sets everything to 0
            
        - list_foreach(Type, list, lambda)
            Iterates over all elements in the List and calls lambda with each as argument.
            Type should be the type of items in the list
            list is the list to iterate over
            lambda should be a function or macro taking a single argument of Type
            
----

# DoubleLinkedList.h
Provides an implementation of a doubly linked list. I've called the type BiList, as in Bi-directional List to make it easier to type over and over...
As with any linked list, pushing or popping will be O(1), and it does not implement random indexing. 
The big difference from the normal LinkedList is that this BiList supports pushing/popping at front and back, as well as some reversing operations.

    Exposes functions:
        
        - init_bilist(Type)
            Generates code for a doubly linked list containing type Type, BiList_Type, along with functions for the BiList such as bilist_Type_new()
            
        - bilist_Type_new()
            Returns an empty BiList_Type
            
        - bilist_Type_copy(const BiList_Type*)
            Returns a deep copy of the passed BiList
            
        - bilist_Type_push_front(BiList_Type*, const Type)
            Pushes the passed item to front of the BiList
            
        - bilist_Type_push_back(BiList_Type*, const Type)
            Pushes the passed item to back of the BiList
        
        - bilist_Type_pop_front(BiList_Type*, Type*)
            Copies the first item in the BiList into the Type pointer, and removes it from the BiList. Returns true if the operation was successful
            
        - bilist_Type_pop_back(BiList_Type*, Type*)
            Copies the last item in the BiList into the Type pointer, and removes it from the BiList. Returns true if the operation was successful
            
        - bilist_Type_first(const BiList_Type*, Type*)
            Copies the first element in BiList into the Type pointer, if it exists
            
        - bilist_Type_last(const BiList_Type*, Type*)
            Copies the lase element in BiList into the Type pointer, if it exists
            
        - bilist_Type_to_array(const BiList_Type*)
            Returns a newly allocated array consisting of the items in the BiList, in order
            
        - bilist_Type_to_array_reversed(const BiList_Type*)
            Returns a newly allocated array consisting of the items in the BiList, in reversed order
            
        - bilist_Type_hash(const BiList_Type*)
            Returns 32 bit hash generated by xoring the murmur-hashes of each item together
            
        - bilist_Type_free(BiList_Type*)
            Frees any resources used by the BiList, and sets everything to 0
            
        - bilist_foreach(Type, list, lambda)
            Iterates over the BiList and calls lambda with each element in order.
            Type should be the type of the elements stored in the list.
            list is the list to iterate over
            lambda should be a function or macro taking a single parameter of Type
        
        - bilist_foreach_reverse(Type, list, lambda)
            Iterates over the BiList and calls lambda with each element in reverse order.
            Type should be the type of the elements stored in the list.
            list is the list to iterate over
            lambda should be a function or macro taking a single parameter of Type
            
----

# Stack.h
A last in first out Stack. Pushing and popping will always be O(1). Does not implement random indexing.

    Exposes functions:
        
        - init_stack(Type)
            Generates code for Stack_Type, along with functions for it. E.g init_stack(int) would generate code for Stack_int along with functions such as stack_int_new()
            
        - stack_Type_new()
            Returns an empty Stack_Type
        
        - stack_Type_copy(const Stack_Type*)
            Returns a deep copy of the passed Stack
            
        - stack_Type_push(Stack_Type*, const Type)
            Pushes the passed item to top of sSack
            
        - stack_Type_pop(Stack_Type*, Type*)
            Pops the top element off the Stack and copies it into the passed Type pointer.
            
        - stack_Type_peek(Stack_Type*, Type*)
            Copies the top element into the passed Type pointer.
            
        - stack_Type_to_array(const Stack_Type*)
            Returns a newly allocated array containing all the elements of the Stack, in order top to bottom
            
        - stack_Type_hash(const Stack_Type*)
            Returns 32 bit hash computed by xoring the murmur-hashes of each item in the Stack
            
        - stack_Type_free(Stack_Type*)
            Frees any resources used by the Stack, and set everything to 0
            
        - stack_foreach(Type, stack, lambda)
            Iterates over all the elements in stack and calls lambda with each, in order top to bottom.
            Type should be the type of element in Stack.
            stack is the stack to iterate over.
            lambda should be a function or lambda taking one parameter of Type.
            
----

# PriorityQueue.h
A priority queue implemented as a min heap. Will keep the minimal element, as determined by the comparison function used, at the top.

    Exposes functions:
        
        -init_heap(Type, comp_func)
            Macro to generate code for Heap_Type, and functions for it. E.h init_heap(int) would generate Heap_int, and functions such as heap_int_new()
            Type should be the type to be stored in the Heap.
            comp_func should be a function or macro taking two arguments a, and b of Type and return: negative if a  < b, zero if a == b, positive if a > b
            
        - heap_Type_new()
            Returns an empty Heap_Type
            
        - heap_Type_copy(const Heap_Type*)
            Returns a deep copy of the passed Heap_Type
            
        - heap_Type_float(Heap_Type*, uint32_t)
            Really only intended for use within the heap, the heap will do all the needed floating itself...
            Has the item specified by the heap-index of the second arg float to its appropriate place in the heap.
            Note that the heap is indexed from 1 internally
            
        - heap_Type_sink(Heap_Type*, uint32_t)
            Really only intended for use within the heap, the heap will do all the needed sinking itself...
            Has the item specified by the heap-index of the second arg sink to its appropriate place in the heap.
            Note that the heap is indexed from 1 internally
            
        - heap_Type_reheap(Heap_Type*)
            If for some reason you mess up the heap internally, this function can be used to restore its properties as a min heap
            
        - heap_Type_from_array(const Type*, const uint32_t)
            Returns a Heap_Type from a deep copy of the passed Type array.
            Number of elements in array specified by second arg
            
        - heap_Type_hash(const Heap_Type*)
            Returns 32 bit murmuhash generated from xoring the hashes of all elements together
            
        - heap_Type_expand(Heap_Type*)
            Should not be used, the Heap will expand as needed, but it is there...
            Doubles the underlying capacity of the Heap
            
        - heap_Type_push(Heap_Type*, const Type)
            Pushes the passed item to the Heap, while maintaining heap order
            
        - heap_Type_pop(Heap_Type*, Type*)
            Pops the minimal element off the Heap and copies it into the passed Type pointer.
            Returns true if an element was popped.
            
        - heap_Type_peek(const Heap_Type*, Type*)
            Copies the minimal element in the Heap into the passed Type pointer
            
        - heap_Type_free(Heap_Type*)
            Frees the underlying array, and sets everything to 0
            
        - heap_foreach(Type, heap, lambda)
            Iterates over all elements in the Heap and calls lambda passing each element as argument.
            Type is not actually needed, it is only here for consistency with the other data types.
            heap is the Heap_Type to iterate over.
            lambda should be a function or macro taking a single parameter of Type
            
----

# HashMap.h
A HashMap implementation. Most operations should be O(1) in the general case. 

    Exposes functions:
    
        - init_hashmap(Key, Val, key_equals, key_hash)
            Macro to generate code for HashMap_Key_Val, where Key is the type of the keys, and Val is the type of the Values.
            key_equals should be a function or macro taking two parameters of type Key and return true if they are deemed equal.
            key_hash should be a function or macro taking a single parameter of type Key and return a 32 bit hash value.
            As one could easily figure out, key equals will be used to test equality between keys, and key_hash will be used for indexing.
            
        - hashmap_Key_Val_new()
            Returns an empty HashMap_Key_Val
            
        - hashmap_Key_Val_copy(const HashMap_Key_Val*)
            Returns a deep copy of the passed HashMap_Key_Val
            
        - hashmap_Key_Val_expand(HashMap_Key_Val*)
            Should not be used, the HashMap will expand on its own when needed, but it is there...
            Will double the number of buckets in the underlying structure.
            
        - hashmap_Key_Val_put(HashMap_Key_Val, const Key, const Val)
            Will store an entry for the passed Key along with the Val in the HashMap.
            If such an entry already exists, the existing Val will be overwritten.
            
        - hashmap_Key_Val_get(const HashMap_Key_Val*, const Key, Val*)
            If an entry for the passed Key exists, the corresponding Val will be copied into the passed Val pointer. Returns true if an entry was found.
            
        - hashmap_Key_Val_contains(const HashMap_Key_Val*, const Key)
            Returns true if HashMap contains an entry for Key
            
        - hashmap_Key_Val_remove(HashMap_Key_Val*, const Key, Val*)
            Will remove the entry in HashMap for Key. If it exists, the corresponding Val will be copied into the passed Val pointer, if the pointer is valid.
            Returns true if operation was successful.
            
        - hashmap_Key_Val_to_array(const HashMap_Key_Val*, Key*, Val*)
            Will copy keys and their respective values into the passed Key and Val pointers. Will only copy anything if the pointers are valid.
            If only one Key pointer is valid, only keys will be copied. Same goes if only the Val pointer is valid.
            Meaning this is a unified function for getting either a Key array, a Val array, or both.
            
        - hashmap_Key_Val_free(HashMap_Key_Val*)
            Frees any resources used by the Hashmap and sets everything to 0
            
        - hashmap_foreach(Key, Val, map, lambda)
            Iterates all the entries in the map and calls lambda with each key and value.
            Key should be the type of the keys.
            Val should be the type of the values.
            map is the map to be iterated over.
            lambda should be a function or macro taking two parameters, one of type Key, and the other of type Val.
            *General tip*: if you only want to use either of key or value, you can simply pass a lambda macro that ignores the other.
            
----

# HashSet.h
A HashSet implementation. Most operations should be O(1) in the general case.

    Exposes functions:
    
        - init_hashset(Type, equals, hash)
            Macro to generate code for HashSet_Type.
            Type should be the type to be contained in the HashSet.
            equals should be a function or macro taking a two parameters of Type returning true if they are to be deemed equal.
            hash should be a function or macro taking a single parameter of Type and return a 32 bit hash value.
            
        - hashset_Type_new()
            Returns an empty HashSet_Type
            
        - hashset_Type_copy(const HashSet_Type*)
            Returns a deep copy of the passed HashSet_Type
            
        - hashset_Type_expand(HashSet_Type*)
            Should not be used, the HashSet will expand on its own when needed, but it's there...
            Will double the number of underlying buckets.
            
        - hashset_Type_put(HashSet_Type*, const Type)
            Puts the passed item into the HashSet, only if it it does not already exist.
            
        - hashset_Type_remove(HashSet_Type*, const Type)
            Removes the passed item from the HashSet, if it exists. Returns true if it was successfully removed.
            
        - hashset_Type_contains(const HashSet_Type*, const Type)
            Returns true if the passed item is already in the HashSet
            
        - hashset_Type_to_array(const HashSet_Type*, Type *arr)
            Copies all elements in HashSet into the Type array pointer passed as argument.
            
        - hashset_Type_free(HashSet_Type*)
            Frees any resources used by the HashSet, and sets everything to 0
            
        - hashset_foreach(Type, set, lambda)
            Iterates through all elements in the HashSet and calls lambda passing each element as argument.
            Type should be the type of elements in the HashSet.
            set is the HashSet to iterate over.
            lambda should be a function or macro taking a single argument of Type.
            
----

# String.h
A String implementation, with short string optimization. Any string that is less than 16 chars (or 16 including the null terminator) will be stored in place, 
I could have gone up to 23, but then I cannot guarantee that it works regardless of endianness. This file exposes the struct String along with functions for it.

    Exposes functions:
        
        - string_new()
            Returns empty String
        
        - string_chars(String*)
            Returns the underlying char array of the passed String
            
        - string_print(const String*)
            Will write the contents of the passed String to stdout
            
        - string_hash(const String*)
            Returns 32 bit murmuhash of the underlying char array
            
        - string_free(String*)
            If any memory has been dynamically allocated by the String, it is freed, and the String is reset to its empty state.
            
        - string_copy(const String*)
            Returns a deep copy of the passed String
        
        - string_from_chars(const char*)
            Returns a String that will contain a deep copy of the passed char array
            
        - string_from_file(const char*)
            Tries to open the file name passed as argument. On success it returns a String with the contents of the file.
            If it fails to open the file, an empty String is returned.
            
        - string_write_to_file(const String*, const char*)
            Tries to open the file name passed to write. The file will be cleared before write, and it will be created if it does not exist.
            If the file cannot be opened or created, nothing happens.
            
        - string_reserve_short(String*, const uint64_t)
            Should not be used, the String will expand as neccessary, but it's there...
            If you really feel the need to use it, make sure the string passed is a short string, i.e flag == 0
            Will allocate a new underlying char array with capacity equal to the first power of 2 greater than the passed minimum size.
            
        - string_reserve_long(String*, const uint64_t)
            Should not be used, the String will expand as neccessary, but it is there...
            If you really feel the need to use this, make sure the String passed is a long string, i.e flag == 1
            Will allocate a new underlying char array with capacity equal to the first power of 2 greater than the passed minimum size
            
        - string_append_chars(String*, const char*)
            Will append the char string passed to the String passed
            
        - string_append_string(String*, const String*)
            Will append the contents of the const String to the String
        
        - string_append(String*, const char)
            Will append the char to String
            
        - string_contains_chars(const String*, const char*)
            Returns true if the pattern specified by the char array is found in the String
            
        - string_contains_string(const String*, const String*)
            Returns true if the pattern specified by the Second String argument is found in the first String argument
            
        - string_compare_chars(const String*, const char*)
            Returns a negative number if String is lexicographically less than chars
            Returns zero if they are equals
            Returns a positive number if chars is lexicographically less than String
            
        - string_compare_string(const String*, const String*)
            Returns a negative number if first is lexicographically less than second
            Returns zero if arguments are equal
            Returns a positive number if second is lexicographically less than first
            
----

# string_utils.h
A small collection of string related functions, works with raw char arrays. This is no functionality that does not already exist in the standard library. 
I just added this because I enjoy making stuff myself, and to experiment with algorithms.
    
    Exposes functions:
        
        - string_length(const char*)
            Returns the length of the char string passed. Assumes the passed string is null-terminated.
            
        - string_compare(const char*, const char*)
            Returns a negative number if first arg is lexicographically less than second arg
            Returns zero if arguments are equal
            Returns a positive number if second arg is lexicographically less than first arg
            Assumes both strings are null-terminated
            
        - string_contains(const char*, const char*)
            Returns true if the pattern specified by the second argument is found within the first arg.
            Assumes both strings are null-terminated
            
----

# memory.h
I added this mostly to get a small include file containing definitions for the basic memory allocation functions (malloc, calloc, free).
This header also includes an implementation of a memcpy function.

    Exposes function:
    
        - copy(void*, const void*, const size_t)
            Copies raw contents from the second pointer into the first.
            Number of bytes copied specified by the second argument.
            
----

# print.h
This header features a small implementation of a printf-like function.
It can be used exactly like printf, except it implements a different set of type specifiers:

  - %c -> a char
  - %s -> a char string
  - %d -> an int
  - %D -> a long int
  - %x -> unsigned int, parsed as hex
  - %X -> unsigned long, parsed as hex
  - %u -> unsigned int
  - %U -> unsigned long
  - %f -> float/double
  - %% -> %
  
  Beware: as of yet i have not implemented any safety in this function. If the message exceeds the buffer size (default 8192) things will go bad...
  
        Exposes functions:
        
            - print(const char*, ...)
                The printf-like function described above.
                
            - println(const char*, ...)
                Macro that simply adds a newline to whatever string is passed, since at least I often forgets newlines otherwise
                
            - reverse(unsigned char*, unsigned char*)
                Only really intended to be used within print, but may be useful in other cases anyway...
                Will reverse the segment of a string bounded by the argument pointers
                
            - parse_int(unsigned char*, long)
                Only really intended to be used within print, but may be useful in other cases anyway...
                Will parse the passed long to string and append it to the char array.
                Returns number of characters appended
                
            - parse_uint(unsigned char*, unsigned long)
                Only really intended to be used within print, but may be useful in other cases anyway...
                Will parse the passed unsigned long to string and append it to the char array.
                Returns number of characters appended
                
            - parse_uint_hex(unsigned char*, unsigned long)
                Only really intended to be used within print, but may be useful in other cases anyway...
                Will parse the passed unsigned long to hex string, and append it to the char array
                Returns number of characters appended
                
            - uintlog10(uint32_t)
                Only really intended to be used within print, but may be useful in other cases anyway...
                Returns the integer log10 of the 32bit uint passed
                
            - print_leading_zero(unsigned char*, double)
                Only really intended to be used within print, but may be useful in other cases anyway...
                Appends appropriate number of zeros before the acual digits in the double arg begins.
                Assumes the double arg < 1.0
                Returns number of characters appended
                
            - parse_uint_no_zerotrail(unsigned char*, uint32_t)
                Only really intended to be used within print, but may be useful in other cases anyway...
                Will parse the passed uint32 into string, but will skip the low zeroes, and appends it to the char array
                Returns the number of characters appended
                
            - parse_double(unsigned char*, double)
                Only really intended to be used within print, but may be useful in other cases anyway...
                Will parse the passed double to string, and append it to the char array.
                It will parse a maximum of 9 digits on either side of the decimal point.
                If the double is greater than 1e10 or less than 1e-10 it will print it using scientific notation with at most 9 decimal places.
                Returns number of characters appended
                
----

# Hash.h
This header exposes three basic hash functions for generic data.

    Exposes functions:
    
        - murmur(const void*, const uint32_t)
            Returns the murmur3 hash from the raw data in the void pointer.
            Number of bytes specified by second argument.
            For the record: this is implemented exactly as Austin Appleby's original implementation, with minor changes to give it my own code-style
            
        - fnv_32(const void*, const uint32_t)
            Returns 32 bit FNV_1a hash from the raw data in the void pointer.
            Number of bytes specified by second argument.
            
        - fnv_64(const void*, const uint32_t)
            Returns 64 bit FNV_1a hash from the raw data in the void pointer.
            Number of bytes specified by second argument.
    
# TODO
  - Some kind of Tree Map
  - Some kind of Tree Set
  - Make print safe
