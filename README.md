```
#include <iostream>
#include <deque> 
using namespace std;

void printKMax(int arr[], int n, int k){
    deque<int> d;
	int i = 0, maxInt = 0;
    while (d.size() < k) {
        d.push_back(arr[i]);
        maxInt = max(maxInt, arr[i]);
        i++;
    }
    while (i < n) {
        cout << maxInt << " ";
        if (arr[i] >= maxInt) {
            maxInt = arr[i];
            
        } else {
            if (d.front() == maxInt) {
                d.push_back(arr[i]);
                d.pop_front();
                maxInt = 0;
                for (int& num : d) {
                    maxInt = max(maxInt, num);
                }
                i++;
                continue;
            }
        }
        d.push_back(arr[i]);
        d.pop_front();
        i++;
    }
    cout << maxInt << endl;
}

int main(){
  
	int t;
	cin >> t;
	while(t>0) {
		int n,k;
    	cin >> n >> k;
    	int i;
    	int arr[n];
    	for(i=0;i<n;i++)
      		cin >> arr[i];
    	printKMax(arr, n, k);
    	t--;
  	}
  	return 0;
}
```
