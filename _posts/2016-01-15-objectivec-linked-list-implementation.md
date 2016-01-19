---
layout: post
title: "Objective-C Implementation of a Linked List"
date: 2016-01-15
---

# Simple Objective-C Implementation of a Linked List

In prepping for a couple of technical interviews I decided that I needed to come up with an implementation of a singly linked list that was not overwhelmingly robust and that could be coded in a matter of minutes.

My implementation only allows for one basic operation: `pushBack`, although additional operations can be added relatively simply, as needed.

Below is a description of the implementation, and everything should be coded in a single objective-c `main.m` file.

So, let's go ahead and get started.

### Node Class

Before implementing the linked list we'll first need to create a `Node` class, since a linked list is essentially a list of connected nodes.

The `Node` class for a singly linked list really only needs two variables: an `obj` variable that will store the value of the node, and a `next` variable that will point to the next node in the list.

The interface for the `Node` is as follows:

**Node Interface**

~~~objective-c
@interface Node : NSObject
@property (nonatomic, strong) id obj;
@property (nonatomic, strong) Node *next;

- (id)init;
@end
~~~

The implementation of the `Node` class is straight forward: we just need to create an `init` method to initialize an empty node.

**Node Implementation**

~~~objective-c
@implementation Node
@synthesize obj;
@synthesize next;

- (id)init {
    obj = nil;
    next = nil;
    return self;
}
@end
~~~

### Linked List Class

Once you have the `Node` class written out, you'll be able to use it in your linked list implementation.

Since we're trying to create a singly linked list implementation that is as simple as possible, we'll only need to create two variables and a single method (along with the `init` method).

The `head` of the list will be in charge of keeping track of the first node in the list, while the `tail` will keep track of the last node in the list.

The `pushBack` method will simply initialize a new node using the passed in value and add it to the **end** of the list.

**Linked List Interface**

~~~objective-c
@interface LinkedList : NSObject
@property (nonatomic, strong) Node *head;
@property (nonatomic, strong) Node *tail;

- (id)init;
- (void)pushBack:(id)anObject;
@end
~~~

The key to the implementation of our Linked List is the `pushBack` method. What we're essentially doing there is initializing a new `Node` and then checking whether or not the linked list already has nodes in it (aka if `head` is `nil`). 

If the linked list is empty, we simply set the newly created `Node` to be both the `head` and the `tail` of the linked list. We do this because at this point the `Node` is both the first and last element of the list.

If, on the other hand, nodes already exist in the linked list (aka `head` is not `nil`), we need to add the newly created `Node` to the end of the list. We do this by first setting the current `tail` in the linked list to point to the newly created `Node`. Then, since the current `tail` is no longer the last element in the list (now the newly created `Node` is), we need to set the `tail` of the linked list to point to the newly created `Node`.

**Linked List Implementation**

~~~objective-c
@implementation LinkedList
@synthesize head;
@synthesize tail;


- (id)init {
    if ((self = [super init]) == nil)
        return nil;
    head = nil;
    tail = nil;
    return self;
}

- (void)pushBack:(id)anObject {
    if (anObject == nil) return;
    
    Node *temp = [[Node alloc] init];
    temp.obj = anObject;
    temp.next = nil;
    
    if (self.head == nil) {
        self.head = temp;
        self.tail = temp;
    } else {
        self.tail.next = temp;
        self.tail = temp;
    }
}
@end
~~~

That's it for the actual implementation! 

Now, to use the linked list you just have to initialize it and add elements to it, which is super simple.

Initializing works the same as with any other object:`LinkedList *list = [[LinkedList alloc] init];`

And in order to add elements to the back of a linked list, simply call `pushBack` on an instance of `LinkedList`, passing in the object value: `[list pushBack: @"a"];`

Putting it all together, your single `main.m` file should look like this:

**`main`.m file**

~~~objective-c
#import <Foundation/Foundation.h>

// Node Class
@interface Node : NSObject
@property (nonatomic, strong) id obj;
@property (nonatomic, strong) Node *next;

- (id)init;
@end

@implementation Node
@synthesize obj;
@synthesize next;

- (id)init {
    obj = nil;
    next = nil;
    return self;
}
@end


// Linked List Class
@interface LinkedList : NSObject
@property (nonatomic, strong) Node *head;
@property (nonatomic, strong) Node *tail;

- (id)init;
- (void)pushBack:(id)anObject;
@end

@implementation LinkedList
@synthesize head;
@synthesize tail;

- (id)init {
    if ((self = [super init]) == nil)
        return nil;
    head = nil;
    tail = nil;
    return self;
}

- (void)pushBack:(id)anObject {
    if (anObject == nil) return;
    
    Node *temp = [[Node alloc] init];
    temp.obj = anObject;
    temp.next = nil;
    
    if (self.head == nil) {
        self.head = temp;
        self.tail = temp;
    } else {
        self.tail.next = temp;
        self.tail = temp;
    }
}
@end

// Main method
int main (int argc, const char * argv[])
{
    @autoreleasepool {
    	 // Here's an example of a linked list initialization
        LinkedList *list = [[LinkedList alloc] init];
        [list pushBack: @"a"];
        [list pushBack: @"b"];
        [list pushBack: @"c"];
        [list pushBack: @"d"];
        [list pushBack: @"e"];
    }
    return 0;
}
~~~

You should be able to implement this on the spot relatively quickly and without much thought and trouble. See below if you want to add some more functionality.

### Useful Additions
#### Printing the List
What good would having a linked list be if you can't really see what you're working with? Below is a simple method you can add to your `@implementation` of `LinkedList` to be able to print out and view the contents of the list. Just make sure you introduce it in your `@interface` first, using: `- (void)print;`

**Print**

~~~objective-c
- (void)print
{
    Node *itr = self.head;
    while (itr != nil) {
        itr.next != nil ? printf("%s -> ", [[itr.obj description] UTF8String]) : printf("%s\n", [[itr.obj description] UTF8String]);
        itr = itr.next;
    }
}
~~~
