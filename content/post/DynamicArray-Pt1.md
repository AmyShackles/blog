---
title: "#100DaysOfCrackingTheCodingInterview: Day 2 - DynamicArray Pt1"
date: 2020-10-14T19:32:21-07:00
lastmod: 2020-10-14T19:32:21-07:00
draft: false
tags: ["data structures", "array"]
categories: ["JavaScript", "Data Structures"]
---

For day 2 of #100DaysOfCrackingTheCodingInterview, I dug in to implementing an array structure.  Rather than just write the traditional push/pop/find methods, I decided to stretch myself and try to implement all of the methods available on JavaScript arrays.

<!--more-->

# The Code So Far

```javascript

class DynamicArray {
    constructor(capacity = 10) {
        this.length = 0;
        this.data = {};
        this.capacity = capacity
    }
    push(...values) {
        let num = values.length;
        if (num + this.length >= this.capacity) {
            this.capacity *= 2;
        }
        while (num > 0) {
            this.data[this.length++] = values[values.length - num];
            num--;
        }
        return this.length;
    }
    pop() {
        return this.data[--this.length];
    }
    unshift(...values) {
        let num = values.length;
        if (num + this.length >= this.capacity) {
            this.capacity *= 2;
        }
        let length = this.length - 1;
        let newArray = {};
        while (length >= 0) {
            newArray[length + values.length] = this.data[length--]
        }

        let index = 0;
        while (index < num) {
            newArray[index] = values[index++];
            this.length++;
        }
        this.data = newArray;
        return this.length;
    }
    shift() {
        if (this.length > 0) {
            let first = this.data[0];
            for (let i = 1; i < this.length; i++) {
                this.data[i - 1] = this.data[i];
            }
            delete this.data[--this.length];
            return first;
        }
        return undefined;
    }
    reverse() {
        for (let i = 0, j = this.length - 1; i < this.length / 2; i++ , j--) {
            let temp = this.data[i];
            this.data[i] = this.data[j];
            this.data[j] = temp;
        }
        return this.data;
    }
    *entries() {
        let next = 0;
        while (next < this.length) {
            yield [ next, this.data[next++]]
        }
    }
    *keys() {
        let next = 0;
        while (next < this.length) {
            yield next++;
        }
    }
    *values() {
        let next = 0;
        while (next < this.length) {
            yield  this.data[next++];
        }
    }
    every(callback, thisArgument) {
        let currentThis, index;
        if (this == null) {
            throw new TypeError("this is null or not defined")
        }
        // If callback isn't callable, throw error
        if (typeof callback !== 'function' && Object.prototype.toString.call(callback) !== '[object Function]') {
            throw new TypeError();
        }

        // Set the value of 'this' if it has been passed in
        if (arguments.length > 1) {
            currentThis = thisArgument;
        }

        index = 0;

        while (index < this.length) {
            let value;

            if (index in this.data) {
                let result;
                value = this.data[index];
                if (currentThis) {
                    result = callback(currentThis, value, index);
                } else {
                    result = callback(value, index);
                }
                if (!result) {
                    return false;
                }
            }
            index++;
        }
        return true;
    }
    some(callback, thisArgument) {
        let currentThis, index;
        if (this == null) {
            throw new TypeError("this is null or not defined")
        }
        // If callback isn't callable, throw error
        if (typeof callback !== 'function' && Object.prototype.toString.call(callback) !== '[object Function]') {
            throw new TypeError();
        }

        // Set the value of 'this' if it has been passed in
        if (arguments.length > 1) {
            currentThis = thisArgument;
        }

        index = 0;

        while (index < this.length) {
            let value;

            if (index in this.data) {
                let result;
                value = this.data[index];
                if (currentThis) {
                    result = callback(currentThis, value, index);
                } else {
                    result = callback(value, index);
                }
                if(result) {
                    return true;
                }
            }
            index++;
        }
        return false;
    };
    filter(callback, thisArgument) {
        let currentThis, index;
        if (this == null) {
            throw new TypeError("this is null or not defined")
        }
        // If callback isn't callable, throw error
        if (typeof callback !== 'function' && Object.prototype.toString.call(callback) !== '[object Function]') {
            throw new TypeError();
        }

        // Set the value of 'this' if it has been passed in
        if (arguments.length > 1) {
            currentThis = thisArgument;
        }

        index = 0;
        let newArr = []
        while (index < this.length) {
            let value;

            if (index in this.data) {
                let result;
                value = this.data[index];
                if (currentThis) {
                    result = callback(currentThis, value, index);
                } else {
                    result = callback(value, index);
                }
                if (result) {
                    newArr.push(value)
                }
            }
            index++;
        }
        return newArr;
    }
    includes(value, fromIndex = 0) {
        if (fromIndex >= this.length) {
            return false;
        }
        let index;
        if (fromIndex < 0) {
            index = this.length + fromIndex;
        } else {
            index = fromIndex;
        }
        while (index < this.length) {
            if (this.data[index++] === value) {
                return true;
            }
        }
        return false;
    }
    find(callback, thisArgument) {
        let currentThis, index;
        if (this == null) {
            throw new TypeError("this is null or not defined")
        }
        // If callback isn't callable, throw error
        if (typeof callback !== 'function' && Object.prototype.toString.call(callback) !== '[object Function]') {
            throw new TypeError();
        }

        // Set the value of 'this' if it has been passed in
        if (arguments.length > 1) {
            currentThis = thisArgument;
        }

        index = 0;
        while (index < this.length) {
            let value;

            if (index in this.data) {
                let result;
                value = this.data[index];
                if (currentThis) {
                    result = callback(currentThis, value, index);
                } else {
                    result = callback(value, index);
                }
                if (result) {
                    return value;
                }
            }
            index++;
        }
        return undefined
    }
    findIndex (callback, thisArgument) {
        if (this.length === 0) return -1;
        let currentThis, index;
        if (this == null) {
            throw new TypeError("this is null or not defined")
        }
        // If callback isn't callable, throw error
        if (typeof callback !== 'function' && Object.prototype.toString.call(callback) !== '[object Function]') {
            throw new TypeError();
        }

        // Set the value of 'this' if it has been passed in
        if (arguments.length > 1) {
            currentThis = thisArgument;
        }

        index = 0;
        let newArr = []
        while (index < this.length) {
            let value;

            if (index in this.data) {
                let result;
                value = this.data[index];
                if (currentThis) {
                    result = callback(currentThis, value, index);
                } else {
                    result = callback(value, index);
                }
                if (result) {
                    return index;
                }
            }
            index++;
        }
        return -1;
    }
    indexOf(value, fromIndex = 0) {
        if (fromIndex >= this.length) return -1;
        let index;
        if (fromIndex >= 0) {
            index = fromIndex;
        } else if (fromIndex < 0) {
            index = this.length + fromIndex;
        }
        while (index < this.length) {
            if (this.data[index] === value) {
                return index;
            }
            index++;
        }
        return -1;
    }
    lastIndexOf(value, fromIndex = this.length - 1) {
        let index;
        if (fromIndex < 0) {
            index = this.length + fromIndex;
        } else if (fromIndex >= this.length) {
            index = this.length - 1;
        } else {
            index = fromIndex;
        }
        if (index < 0) return -1;
        while (index >= 0) {
            if (this.data[index] === value) {
                return index;
            }
            index--;
        }
        return -1;
    }
    concat(...arrays) {
        if (arrays.length === 0) {
            return {...this.data}
        }
        let newArr = new DynamicArray();
        const values = [...this.values()];
        newArr.push(...values)
        for (let i = 0; i < arrays.length; i++) {
            if (arrays[i].length > 1) {
                newArr.push(...arrays[i])
            } else {
                newArr.push(arrays[i])
            }
        }
        return newArr;
    }
}
```

# The Tests So Far

```javascript
const { DynamicArray } = require('./index.js');
const expect = require('chai').expect;

describe('DynamicArray', () => {
    describe('push', () => {
        it('should add a value to the end of the array', () => {
            let arr = new DynamicArray();
            arr.push(3, 4);
            const actual = arr;
            const expected = {
                length: 2,
                data: {0: 3, 1: 4},
                capacity: 10
            };
            expect(actual).to.deep.equal(expected);
        });
        it('should return the new length of the array', () => {
            let arr = new DynamicArray();
            const actualLength = arr.push(3, 4);
            const expectedLength = 2;
            expect(actualLength).to.equal(expectedLength);
        })
        it('should double the capacity if the length meets or exceeds capacity', () => {
            let arr = new DynamicArray(2);
            arr.push(2, 3);
            const actual = arr.capacity;
            const expected = 4
            expect(actual).to.equal(expected)
        })
    })
    describe('pop', () => {
        describe('should remove the last element from the list and return it', () => {
            it('should work for integers', () => {
                let arr = new DynamicArray();
                arr.push(1,2,3,4,5);
                const actual = arr.pop();
                const expected = 5;
                expect(actual).to.equal(expected)
            })
            it('should work for strings', () => {
                let arr = new DynamicArray();
                arr.push('One', 'for', 'the', 'money');
                const actual = arr.pop();
                const expected = 'money';
                expect(actual).to.equal(expected)
            })
            it('should work for objects', () => {
                let arr = new DynamicArray();
                arr.push({doe: 'a deer'}, {ray: 'a drop of golden sun'}, {me: 'a name I call myself'}, { fa: 'a long, long way to run'});
                const actual = arr.pop();
                const expected = { fa: 'a long, long way to run'}
                expect(actual).to.deep.equal(expected)
            })
        })
    })
    describe('shift', () => {
        describe('should remove the first element of the list and return it', () => {
            it('should work for integers', () => {
                let arr = new DynamicArray();
                arr.push(1,2,3,3,4);
                const actual = arr.shift();
                const expected = 1;
                expect(actual).to.equal(expected)
            })
            it('should work for strings', () => {
                let arr = new DynamicArray();
                arr.push('One', 'is', 'the', 'loneliest', 'number');
                const actual = arr.shift();
                const expected = 'One';
                expect(actual).to.equal(expected)
            })
            it('should work for objects', () => {
                let arr = new DynamicArray();
                arr.push({love: 'love me do'}, {you: 'know I love you'});
                const actual = arr.shift();
                const expected = { love: 'love me do' };
                expect(actual).to.deep.equal(expected)
            })
        })
    })
    describe('unshift', () => {
        it('should add values to the start of the array', () => {
            let arr = new DynamicArray();
            arr.push(8,9,10);
            arr.unshift(1,2);
            const actual = arr;
            const expected = {
                length: 5,
                capacity: 10,
                data: {
                    0: 1,
                    1: 2,
                    2: 8,
                    3: 9,
                    4: 10,
                }
            };
            expect(actual).to.deep.equal(expected);
        })
        it('should return the new length of the array', () => {
            let arr = new DynamicArray();
            arr.push(3);
            const actual = arr.unshift(7,8,9);
            const expected = 4;
            expect(actual).to.equal(expected)
        })
        it('should double the capacity if the length meets or exceeds capacity', () => {
            let arr = new DynamicArray();
            arr.unshift(1,2,3,4,5,6,7,8,9,10,11);
            const actual = arr.capacity;
            const expected = 20;
            expect(actual).to.equal(actual)
        })
    })
})
```