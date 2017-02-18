'''
Assumptions:
1.If there are 2 soldiers who share the same maximum value and the same minimum value in B, 
then the program returns any of the two soldier's indexes.
'''
'''
    A segment tree is built here, Each node stores the left and right
    endpoint of an interval and the maximum value in the interval.
    All of the leaves will store elements of the array and each internal 
    node will store maximum value of leaves under it.
    Creating the tree takes O(n) time. Querytakes O(log n).
'''

import math

'''A Node class that stores the values initiated in it '''

class Node(object):
    def __init__(self, start, end):
        self.start = start
        self.end = end
        self.left = None
        self.right = None
        self.max = -math.inf
        self.maxIndex = -math.inf
        
        #checks if there are multiple occurences of the max value 
        #under the leaves of the particular node
        #True if they exist and False if there is only one occurence
        self.check = False
        
        #if multiple occurences of the max value occur, stores them 
        #ascending order acc to B values
        self.BValues = (-math.inf,-math.inf)
        
        #holds the max value and the maxIndex
        self.AIndex = (-math.inf,-math.inf)


''' A Class for building the segment tree '''

class NumArray(object):
    def __init__(self, nums, B):

        ''' Initializing data Structure - nums of type Int'''
        
        #Creating the tree from input array
        def createTree(nums, l, r, B):
            
            #Base case
            if l > r:
                return None
                
            #Leaf node
            if l == r:
                n = Node(l, r)
                n.total = nums[l]
                n.max = nums[l]
                n.maxIndex = l
                n.AIndex = (l, r)
                return n
            
            mid = (l + r) // 2
            
            root = Node(l, r)
            
            #Recursively building the Segment tree
            root.left = createTree(nums, l, mid, B)
            root.right = createTree(nums, mid+1, r, B)

            #Finding the maximum value of each node at each level
            if root.left.max > root.right.max:
                root.max = root.left.max
                root.maxIndex = root.left.maxIndex
                root.AIndex = (root.left.maxIndex, root.right.maxIndex)
            elif root.left.max < root.right.max:
                root.max = root.right.max
                root.maxIndex = root.right.maxIndex
                root.AIndex = (root.right.maxIndex, root.left.maxIndex)
            else:
                root.max = root.right.max
                if root.right.check == False and root.left.check == False:    
                    root.check = True

                    if B[root.right.maxIndex] > B[root.left.maxIndex]:
                        root.BValues = (root.left.maxIndex, root.right.maxIndex)
                        root.AIndex = (root.left.maxIndex, root.right.maxIndex)
                    elif B[root.right.maxIndex] <= B[root.left.maxIndex]:
                        root.BValues = (root.right.maxIndex, root.left.maxIndex)
                        root.AIndex = (root.left.maxIndex, root.right.maxIndex)
                    root.maxIndex = root.BValues[0]
                elif root.right.check == True or root.left.check == True:
                    if root.right.check == True:
                        root.maxIndex = min(root.right.AIndex[0], root.right.AIndex[1], root.left.maxIndex)
                    elif root.left.check == True:
                        root.maxIndex = min(root.left.AIndex[0], root.left.AIndex[1], root.right.maxIndex)
                    root.check = True
                    root.AIndex = (root.left.maxIndex, root.right.maxIndex)

                    
            return root
        
        
        self.root = createTree(nums, 0, len(nums)-1, B)
            
    def maxRange(self, i, j):

        ''' Returns a list of shortlisted canditates in the given range '''
        
        def rangeMax(root, i, j):
            
            #If the range exactly matches the root, we already have the maxIndex
            if root.start == i and root.end == j:
                return [[root.max, root.maxIndex, root.check]]
            
            mid = (root.start + root.end) // 2
            
            #If end of the range is less than the mid, the entire interval lies
            #in the left subtree
            if j <= mid:
                return rangeMax(root.left, i, j)
            
            #If start of the interval is greater than mid, the entire inteval lies
            #in the right subtree
            elif i >= mid + 1:
                return rangeMax(root.right, i, j)
            
            #Otherwise, the interval is split.
            #by splitting the interval, we add all the shortlisted soldiers to a list
            else:
                x = rangeMax(root.left, i, mid)
                y = rangeMax(root.right, mid+1, j)
                return x+y

        return rangeMax(self.root, i, j)
        
    def find_Index(self, L, B):
        
        ''' finds the soldier based on the selection criteria:
            1. if there is only one Max A -> returns it
            2. if count of Max A value == 2 -> returns the one with lesser B value
            3. if count of Max A value > 2 -> returns the one with shortest index value
        '''
        def Index(L, B):
            
            #if only one soldier is shortlisted
            if len(L) == 1:
                return L[0][1]+1
            
            maxi = 0
            maxi_Index = 1
            check = 2
            mini_Index = -math.inf
            mini = []
            
            #finding the soldier who matches the selection criteria
            for i in range(len(L)-1):
                if L[i][maxi] != L[i+1][maxi]: 
                    L[i+1] = L[i] if L[i][maxi] > L[i+1][maxi] else L[i+1]
                else:
                    if L[i][check] == False and L[i+1][check] == False: 
                        mini_Index = L[i][maxi_Index]
                        L[i][check] = True
                        mini = L[i] if L[i][maxi_Index] < L[i+1][maxi_Index]  else L[i+1]
                        L[i+1] = L[i] if B[L[i][maxi_Index]] <= B[L[i+1][maxi_Index]] else L[i+1]
                        L[i+1][check] = True
                    elif L[i][check] == True or L[i+1][check] == True: 
                        
                        if mini_Index > -math.inf:
                            L[i+1] = mini
                        else:
                            L[i+1] = L[i] if L[i][1] < L[i+1][1] else L[i+1]
                            L[i+1][check] = True
                
            return L[len(L)-1][1]+1
        return Index(L, B)


'''
    Main function - accepts N, A and B as input from the user
                  - number of wars & the range follows
    For each range, a suitable soldier is found.
'''
N = int(input())
A = list(map(int, input().split()))
B = list(map(int, input().split()))

#Creating an instance of the class, and thus creating the segment tree
numArray = NumArray(A, B)

K = int(input())
for _ in range(K):
    start, end = map(int, input().split())
    L = numArray.maxRange(start-1, end-1)
    print(numArray.find_Index(L, B))
