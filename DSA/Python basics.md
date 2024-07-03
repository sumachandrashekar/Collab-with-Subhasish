# Basics of Python for DSA

## primitives
x = 1             # integer
x = 1.2           # float
x = True          # bool
x = None          # null
x = float('inf')  # infinity (float)

## objects
x = [1, 2, 3] # list
x = (1, 2, 3) # tuple
x = {1: "a"}  # dict
x = {1, 2, 3} # set
x = "abc123"  # str

## Loops with range
for i in range(4):
    # 0 1 2 3

for i in range(1, 4):
    # 1 2 3

for i in range(1, 6, 2): # loop in steps of 2
    # 1 3 5

for i in range(3, -1, -1): # loop backwards
    # 3 2 1 0


## Loops over data structures

arr = ["a", "b"]
for x in arr:
    # "a" "b"

hmap = {"a": 4, "b": 5}
for x in hmap:
    # "a" "b"


# splicing
# splice array using x[start:end:step]
x = [0,1,2,3,4]
x[2:]           # [2,3,4]
x[:2]           # [0,1]
x[1:4]          # [1,2,3] 
x[3::-1]        # [3,2,1,0]
x[-1]           # 4


# List comprehension

x = [2*n for n in range(4)] 
#   [0, 2, 4, 6]

x = [a for a in [1,2,3,4,5,6] if a % 2 == 0]
#   [2, 4, 6]

# 2 x 3 matrix of zeros
x = [[0 for col in range(3)] for row in range(2)]
#   [[0, 0, 0],
#    [0, 0, 0]]


x = [a*b   for a in A   for b in B   if a > 5]

# Is equivalent to:
x = []
for a in A:
    for b in B:
        if a > 5:
            x.append(a*b)

