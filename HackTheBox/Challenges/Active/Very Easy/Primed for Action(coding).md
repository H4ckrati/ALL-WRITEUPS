
Recently, I tackled a challenge from Hack The Box (HTB) titled “Primed for Action.” It was a simple but fun exercise in prime number identification. The challenge provided a list of numbers, and the task was to find two prime numbers in the list, multiply them, and get the key.

> The Challenge:
> 
> _“Intelligence units have intercepted a list of numbers. They seem to be used in a peculiar way — the adversary seems to be sending a list of numbers, most of which are garbage, but two of which are prime. These 2 prime numbers appear to form a key, which is obtained by multiplying the two. Find the key and help us solve the case.”_

### The Solution

To solve the puzzle, I wrote a Python program that:

1. Takes a space-separated list of numbers as input.
2. Identifies which numbers in the list are prime.
3. Multiplies the first two prime numbers it finds and prints the result.

Here’s the code I used to solve the challenge:

# Function to check if a number is prime  
def is_prime(num):  
    if num <= 1:  
        return False  
    for i in range(2, int(num ** 0.5) + 1):  
        if num % i == 0:  
            return False  
    return True  
  
# Take input as a string of space-separated numbers  
n = input()  
  
# Convert the input into a list of integers  
numbers = list(map(int, n.split()))  
  
# Find prime numbers in the list  
prime_numbers = [num for num in numbers if is_prime(num)]  
  
# If exactly two prime numbers are found, calculate their product  
if len(prime_numbers) == 2:  
    product = prime_numbers[0] * prime_numbers[1]  
    print(product)  
else:  
    print("Not enough prime numbers in the list.")

### Conclusion

This challenge was a great opportunity to practice basic number theory and Python programming. By identifying prime numbers in the list and multiplying them, I was able to extract the key from the given data.