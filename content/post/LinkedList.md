---
title: "#100DaysOfCrackingTheCodingInterview: Day 1 - LinkedList"
date: 2020-10-13T23:32:57-07:00
lastmod: 2020-10-13T23:32:57-07:00
draft: false
tags: ["data structures", "linked list"]
categories: ["JavaScript", "Data Structures"]
---

I am tired of being rubbish at technical interviews.  I realized that the only way that I can stop _being_ rubbish at technical interviews is to develop a better understanding of data structures, develop a more friendly relationship with algorithms, and practice.

Today, I made the public commitment to #100DaysOfCrackingTheCodingInterview.  What follows is the implementation I came up with for a LinkedList in JavaScript.

<!--more-->

# Linked List

What is a linked list, you might ask.  It's a good question.  You can think of a linked list as a kind of, well, list.  Each element in the list is a Node, which is a data structure that has both a value and a pointer (reference) to the next element in the list.  We refer to the node at the beginning of a linked list as the head, and it's that first node that we pass around when we're working with a linked list.  If you've worked with tree structures in the past, it's the same concept where any part of a tree is a tree unto itself.

The realization I had today was that you don't actually need two separate classes in order to implement a LinkedList in JavaScript.  I'd previously always defined a Node class and a LinkedList class, but since the only thing I ever used the LinkedList class for was to store the head of the linked list, it's actually not necessary.

# Implementation

```javascript
class LinkedListNode {
    constructor(value = null) {
        this.value = value;
        this.next = null;
        this.length = value !== null ? 1 : 0;
    }
    indexOf(value) {
        let count = 0;
        let current = this;
        while (current !== null) {
            if (current.value === value) {
                return count;
            }
            count++;
            current = current.next;
        }
        return -1;
    }
    insert(value) {
        let newNode = new LinkedListNode(value);
        ++this.length;
        let head = this;
        if (!head.value) {
            head.value = value;
            head.next = null;
            return head;
        }
        let length = this.length;
        while (head.next) {
            head = head.next;
        }
        head.next = newNode;

        return this;
    }
    insertAt(index, value) {
        if (index < 0 || index > this.length) {
            return -1;
        }
        let newNode = new LinkedListNode(value);
        ++this.length;
        let head = this;
        if (index === 0) {
            if (!head.value) {
                head.value = value;
                head.next = null;
                return head;
            } else {
                let next = {...head, length: 1}
                this.value = value;
                this.next = next;
                return this;
            }
        }
        let count = 0;
        while (count < index - 1) {
            head = head.next;
            count++;
        }
        if (count === index - 1) {
            if (head.next) {
                let next = head.next
                let prev = head;
                prev.next = newNode;
                newNode.next = next;
                return this;
            }
        }
    }
    remove(value) {
        let head = this;
        let prev;
        if (head.value === value) {
            if (head.next && head.next.value) {
                let copy = {...head}
                this.value = head.next.value;
                this.next = head.next.next;
                --this.length;
                return this;
            } else {
                delete this.value;
                delete this.next;
                this.length = 0;
                return {};
            }
        }
            while (head.next) {
                prev = head;
                head = head.next;
                if (head.value === value) {
                    prev.next = head.next;
                    --this.length;
                    return this;
                }
            }
        return -1;
    }
    removeFrom(index) {
        let current = this;
        if (index === 0) {
            if (current.next && current.next.value) {
                let head = {...current}
                this.value = current.next.value;
                this.next = current.next.next;
                --this.length;
                return this;
            } else {
                delete this.value;
                delete this.next;
                this.length = 0;
                return {};
            }
        }
        let count = 0;
        while (current.next && count < index - 1) {
            current = current.next;
            count++;
        }
        if (count === index - 1) {
            if (current.next.next) {
                let val = current.next;
                current.next = current.next.next;
                --this.length;
                return this;
            }
        }
        return -1;
    }
    search(value) {
        let head = this;
        if (head.value === value) {
            return head;
        } else {
            while (head.next) {
                head = head.next;
                if (head.value === value) {
                    return head;
                }
            }
        }
        return -1;
    }
    size() {
        return this.length;
    }
    print() {
        let head = this;
        if (!head.value) {
            return;
        }
        let count = 0;
        console.log('\n')
        while (head) {
            console.log('value', head.value, 'at index ', count++);
            head = head.next;
        }
        console.log('NULL')
    }
}
```

# Testing

I used Mocha and Chai to test my implementation.

```javascript
const { LinkedListNode } = require('./index.js');
const expect = require('chai').expect;

describe('LinkedListNode', () => {
    describe('indexOf', () => {
        it('should return the index of the first occurance of the value if the value exists in the linked list', () => {
            let head = new LinkedListNode(5);
            head.insert(2);
            head.insert(3);
            head.insert(3);
            const expected = 2;
            const actual = head.indexOf(3);
            expect(actual).to.equal(expected)
        });
        it('should return -1 if the value does not exist in the linked list', () => {
            let head = new LinkedListNode(2);
            const expected = -1;
            const actual = head.indexOf(7);
            expect(actual).to.equal(expected)
        })
    })
    describe('insert', () => {
        it('should insert a node to the end of a linked list', () => {
            let head = new LinkedListNode(1);
            const actual = head.insert(3);
            const expected = { 
                length: 2, value: 1, next: { 
                    length: 1, value: 3, next: null 
                    }
                }
            expect(actual).to.deep.equal(expected);
        });
        it('should replace the head of the linked list if there is no head', () => {
            let head = new LinkedListNode();
            const actual = head.insert(2);
            const expected = { length: 1, value: 2, next: null };
            expect(actual).to.deep.equal(expected);
        })
    })
    describe('insertAt', () => {
        it('should insert a node at a given index if that index exists in the linked list', () => {
            let head = new LinkedListNode(1);
            head.insert(2);
            head.insert(3);
            const actual = head.insertAt(1, 5);
            const expected = { 
                length: 4, value: 1, next: { 
                    length: 1, value: 5, next: { 
                        length: 1, value: 2, next: { 
                            length: 1, value: 3, next: null 
                            }
                        }
                    }
                };
            expect(actual).to.deep.equal(expected);
        })
        it('should replace the head of the linked list if the index is 0', () => {
            let head = new LinkedListNode(1);
            const actual = head.insertAt(0, 5);
            const expected = {
                length: 2, value: 5, next: {
                    length: 1, value: 1, next: null
                }
            };
            expect(actual).to.deep.equal(expected)
        })
        it('should become the head of the linked list if there is no head and the index is 0', () => {
            let head = new LinkedListNode();
            const actual = head.insertAt(0, 7);
            const expected = {
                length: 1, value: 7, next: null
            };
            expect(actual).to.deep.equal(expected)
        })
        it('should return -1 if the index is less than 0', () => {
            let head = new LinkedListNode(1);
            let actual = head.insertAt(-2);
            expect(actual).to.equal(-1)
        })
        it('should return -1 if the index is larger than the linked list length', () => {
            let head = new LinkedListNode(2);
            let actual = head.insertAt(2, 7);
            expect(actual).to.equal(-1)
        })
    })
    describe('remove', () => {
        it('should return an empty object if it removes the head node and there are no other nodes', () => {
            let head = new LinkedListNode(1);
            const actual = head.remove(1);
            const expected = {};
            expect(actual).to.deep.equal(expected);
        });
        it('should return the new head of the list if it removes the head node and there are other nodes', () => {
            let head = new LinkedListNode(1);
            head.insert(5);
            head.insert(3);
            const actual = head.remove(1);
            const expected = {
                length: 2, value: 5, next: {
                    length: 1, value: 3, next: null
                }
            };
            expect(actual).to.deep.equal(expected);
        })
        it('should return the head of the list with the first occurrence of the value removed if it exists', () => {
            let head = new LinkedListNode(1);
            head.insert(2);
            head.insert(3);
            head.insert(4);
            const actual = head.remove(3);
            const expected = {
                length: 3, value: 1, next: {
                    length: 1, value: 2, next: {
                        length: 1, value: 4, next: null
                    }
                }
            };
            expect(actual).to.deep.equal(expected);
        })
        it('should return -1 if the value does not exist', () => {
            let head = new LinkedListNode(1);
            const actual = head.remove(3);
            const expected = -1;
            expect(actual).to.equal(expected);
        })
    })
    describe('removeFrom', () => {
        it('should return an empty object if it removes the first index and there are no other nodes in the list', () => {
            let head = new LinkedListNode(1);
            const actual = head.removeFrom(0);
            const expected = {};
            expect(actual).to.deep.equal(expected);
        })
        it('should return the head of the new list if it removes the first index and there are other nodes in the list', () => {
            let head = new LinkedListNode(1);
            head.insert(2);
            const actual = head.removeFrom(0);
            const expected = {
                length: 1, value: 2, next: null
            };
            expect(actual).to.deep.equal(expected);
        })
        it('should return the list with the node at the given index removed if that index exists', () => {
            let head = new LinkedListNode(1);
            head.insert(2);
            head.insert(3);
            head.insert(4);
            head.insert(5);
            const actual = head.removeFrom(2);
            const expected = {
                length: 4, value: 1, next: {
                    length: 1, value: 2, next: {
                        length: 1, value: 4, next: {
                            length: 1, value: 5, next: null
                        }
                    }
                }
            }
            expect(actual).to.deep.equal(expected)
        })
        it('should return -1 if the index is less than 0', () => {
            let head = new LinkedListNode(2);
            const actual = head.removeFrom(-2);
            const expected = -1;
            expect(actual).to.equal(expected)
        });
        it('should return -1 if the index is greater than the length of the list', () => {
            let head = new LinkedListNode(1);
            const actual = head.removeFrom(3);
            const expected = -1;
            expect(actual).to.equal(expected)
        })
    });
    describe('search', () => {
        it('should return the first node containing the value', () => {
            let head = new LinkedListNode(1);
            head.insert(2);
            head.insert(4);
            const actual = head.search(2);
            const expected = {
                length: 1, value: 2, next: {
                    length: 1, value: 4, next: null
                }
            };
            expect(actual).to.deep.equal(expected);
        });
        it('should return -1 if the value does not exist', () => {
            let head = new LinkedListNode(1);
            const actual = head.search(4);
            const expected = -1;
            expect(actual).to.equal(expected)
        })
    })
    describe('size', () => {
        it('should return 0 if there is no value used in initializer', () => {
            let head = new LinkedListNode();
            const actual = head.size();
            const expected = 0;
            expect(actual).to.equal(expected);
        })
        it('should return 0 if the head of the list has been removed and there are no other values', () => {
            let head = new LinkedListNode(1);
            head.remove(1);
            const actual = head.size();
            const expected = 0;
            expect(actual).to.equal(expected)
        })
        it('should return the size of the linked list', () => {
            let head = new LinkedListNode(1);
            head.insert(2);
            head.insert(4);
            head.insert(5);
            const actual = head.size();
            const expected = 4;
            expect(actual).to.equal(expected)
        });
        it('should return the size of the linked list even if removals have occurred', () => {
            let head = new LinkedListNode(1);
            head.insert(5);
            head.insert(2);
            head.insert(3);
            head.remove(5);
            head.insert(7);
            head.remove(3);
            const actual = head.size();
            const expected = 3;
            expect(actual).to.equal(expected)
        })
    })
})
```

# Closing Thoughts

I realize I never really explained any of this.  If you'd like to reach out and ask me questions about any of it, feel free to ping me on Twitter @amyshackles 