we have successfully finished binary search and its advanced application. let us now move on to study number theory which is the most important part of CP
## Number_Theory
### EP 47 (binary number and bits basics)

AND (&), OR (|), XOR (^) 
- XOR returns 0 when both are same else 1
- XOR can be use very creatively in alot of problems
left/right shift
- 3 << 2 => 11 -> 1100 (=12)
- 1 << n => 2^n
### EP 48 (playing with bits)
- indexing in binary nos start from right (and 0,1,2... obv)
- right most digit in binary nos is least sig and left most is most sig
- set bit =1 , unset = 0
- to check if nth bit is set or not, take a number with that index bit = 1 and rest 0, then do and of both nos, if u get 0 its not set else its set
- 2^n - 1 form is n 1s in binary
- code for checking if ith bit is set for int a :
```cpp
cout << (a & (1>> i) == 0);
```
- setting ith bit
```cpp
a = a | (1<<i);
```
- unsetting ith bit
```cpp
a = a & (~(1<<i));
```
- toggling ith bit
```cpp
 a = a ^ (1<< i);
```
- printing the binary
```cpp
void printBinary(int n){
	for (int i = 31; i >= 0; --i)
	{
		cout << ((n >> i) & 1);
		//above line checks whether ith bit is 1 or 0
	}
	cout << endl;
	
}
```
- there exists a func in stl for checking set count named __builtin_popcount() or __builtin_popcountll()

### EP 49 (amazing bit tricks)
- checking whether a no is even or odd
```cpp
cout << (n &1 == 0); //returns true for even 
```
- multiply & div by 2^i
```cpp
cout << (n<<i); //mult by 2^i
cout << (n >> i); //div by 2^i

```
- converting upper case/ lower  case using bit toggle. main idea is 5th bit in capital and smaller are the only difference in them so we can toggle to get one another.
```cpp
 string input;
 cin >> input;

 for (int i = 0; i < input.size(); ++i)
 {
 	input[i] = input[i] ^ (1<< 5);
 }
 cout << input;
```
- removing some i least significant digits
```cpp
// i+1 bcz indexing starts from 0
cout << a & (~((1 << i+1) - 1));
```
- removing some  most significant digits on left of ith bit
```cpp
cout << a & ((1<< i+1) - 1);
``` 
- checking if a no. is power of 2
```cpp
if (n & n-1 == 0){
    // power of 2
}
else{
    // no
}
```
### ep 50 (prop of XOR operator)
- x^x = 0, x^0 = x
- order of xor do not matter ie x ^ y ^ z = x ^ z ^ y
- using above 2 we can swap two numbers like below
```cpp
int a = 10, b = 4;
a = a^ b;
b = b ^ a; // now b becomes a
a = a^b // now a becomes b
```
**odd count que**
```cpp
// among n nos we have only one of them ahve oddd count so we take xor of all of them which cancels all even counts.
for (int i = 0; i < n; i++){
    ans = ans ^ array[i];
}
```
### ep 51 (bitmasking)
it is the ability to represent a real world problem as a binary number. (this number is bitmask)
- ex : say u have smth like 2,3 -> 1100 0,1,2 -> 0111 and 1,3 -> 1010. now after representation we can solve variety of problems like taking intersection of n stuff in O(1) like intersec in of 2,3 and 0,1,2 is just and of both which is 0100

now we do a ques from cf blog where we have to max intersection of common days between workers
```cpp
int n;
vector<int> masks(n,0);
cin >> n;
for (int i = 0; i < n; i++){
	int days;
	cin >> days;
	for (int j = 0; j < days; j++){
		int d;
		cin >> d;
		masks[i] = masks[i] | (1 << d);
	}
}

for (int i = 0; i < n; i++){
	for (int j = i+1; j < n; j++){
		// now intersection can be taken in O(1) instead of O(30)
		int intersection = __builtin_popcount((masks[i] & masks[j])); // gives no of set bits
		// now just use a max func to get max
	}
}
```

### ep 52 generating subsets using bitmasking
we did bitmasking last class, its the same concept. jth bit set implies jth index of array is taken.

note time complexity of below code is o(n * (2 ^n))
```cpp
//leetcode subsets
vector<vector<int>> subsets(vector<int>& nums) {
        int n = nums.size();
        int subset_cnt = (1 << n);
        vector<vector<int>> subsets;

        for (int i = 0; i < subset_cnt; i++) {
            vector<int> subset;
            // check which bits are set
            for (int j = 0; j < n; j++) {

                if ((i & (1 << j)) != 0) { // if jth bit is set take that
                                           // element
                    subset.push_back(nums[j]);
                }
            }
            subsets.push_back(subset);
        }
        return subsets;
    }
```
### ep 53 gcd and lcm with euclids algorithm
basics of lcm and gcd. (already know)

implementing gcd using euclids algorithm
```cpp
// below func has roughly log(N) time complexity as we take max divisions which is just log 
int gcd(int a, int b){ // a is dividend, b is divisor so a > b in general
	if (b == 0){
		return a; // obv gcd of b and 0 is b
	}
	return gcd(b, a%b);
}
```
above is based on fact that gcd(a,b) is gcd(b, a%b).

__gcd() is built in func in stl.

gcd of multiple numbers can be computed by taking gcd of smaller part of them then taking gcd with remaining numbers
- ex: gcd(a,b,c) = gcd(a,gcd(b,c)) 


### ep 54 binary exponentiation

the reason we study this is bcz the built in pow function has alot of precision errors since it returns a double. so we need our own funcs.

below is recursive code for bin exp.
```cpp
const int M = 1e9 + 7;
int binexprec(int a, int b){
	if (b == 0) return 1;
	int res = binexprec(a,b/2);
	if (b&1){
		return (a * ((1LL *res * res)%M)) % M;
	}
	else{
		return (1LL *res * res) % M;
	}
}
```

now we look at iterative bin exp which is faster and better than above.

main idea is to write binary of the power b then do below
- ex : 3^5 = 3^(101) = 3^(2^2 + 2^0) = 3^4 * 3^1
```cpp
int binexpiter(int a, int b){
	int ans = 1;
	while(b){
	if (b&1){ //ie current (last) bit is set
		//in this case we consider the power of a
		ans = (1LL*ans * (a))%M; // a is current power of a
	}
	//after this increase a power by square
	a = (1LL*a * a) % M;

	//get the next bit to current
	b = b >> 1;
	}
}
```

### ep 55 large exponentiation

in this lec, we analyse cases for when either a is large or b is large etc.

the code we wrote above assumed that both a and b are in integer range and M is of 1e9 order.

1. let us first see case of a <= 10^18. this case is very trivial as u can see we can directly take mod of a by mod arithematic rules. 
- a^b % M = ((a% M)^b)%M

```cpp
// same func as above just use like below
binexpiter(a%M, b);
```
2. now lt us observe case of M <=10^18. now we cannot do the 1LL * stuff as even this could overflow (bcz M range is high so is the output) here for multiplying any two numbers a*b we have to write a function assuming both a and b are <= 10^18. since max value of a + b is 2 * 10^18 and max we can store in long long is > 2 * 10^18. so we can use a custom multiplication function which converts multiplication to addition then it takes mod every 2 terms hence final value remains <= 10^18. but this would make the mult O(N) we need faster like O(logN) so we use binary multiplication.
```cpp
long long binmult(long long a, long long b){
	long long ans = 0;
	while(b){
	if (b&1){ //ie current (last) bit is set
		//in this case we consider the power of a
		ans = (ans + a)%M; // a is current power of a
	}
	//after this increase a power by square
	a = (a + a) % M;

	//get the next bit to current
	b = b >> 1;
	}
}
```
- this function will allow us to compute a * b % M when both a, b are in range of 10^18. now we put this in our exp code
```cpp
long long binexpiter(long long a, long long b){
	long long ans = 1;
	while(b){
	if (b&1){ //ie current (last) bit is set
		//in this case we consider the power of a
		ans = binmult(ans,a); // a is current power of a
	}
	//after this increase a power by square
	a = binmult(a,a);

	//get the next bit to current
	b = b >> 1;
	}
}
```
### ep 56 large exponentiation with large b
here if b <= 10^18 then we can simply replace int b with long long b in our own function no big deal. let us study what happens when b exceeds 1e18.

in such cases we would use euler theorm which states state that a^b % M is just (a ^ (b % phi(M)))% M.
**note that it is only valid when gcd(a,M) =1**.

- for prime M, we can write a^b % M as (a^ (b % M-1))%M

```cpp
int binexpiter(int a, int b, int m){
	int ans = 1;
	while(b){
	if (b&1){ //ie current (last) bit is set
		//in this case we consider the power of a
		ans = (1LL*ans * (a))%m; // a is current power of a
	}
	//after this increase a power by square
	a = (1LL*a * a) % m;

	//get the next bit to current
	b = b >> 1;
	}
}

// now to compute a^ (b^c), assume phi(m) is etf
binexpiter(a, binexpiter(b,c,phi(m)), m);
```

### ep 57 (factors and divisors)
