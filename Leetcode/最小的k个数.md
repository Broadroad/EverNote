# partition 
划分算法模板要记得

```c++
class Solution {
public:
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        if (k == 0) return {};
        int n = arr.size();
        findKth(arr, 0, n-1, k);
        return vector<int>(arr.begin(), arr.begin()+k);
    }

private:
    int findKth(vector<int>& arr, int s, int e, int k) {
        //cout << s << " " << e << endl;
        if (s == e) return arr[s];
        int position = partition(arr, s, e);
        cout << position << endl;
        if (position + 1 == k) return position;
        else if (position + 1 < k) return findKth(arr, position+1, e, k);
        else return findKth(arr, s, position-1, k);
    }

    int partition(vector<int>& nums, int s, int e) {
        int left = s, right = e;
        int pivot = nums[left];
        // 进行partition
        while (left < right) {
            while (left < right && nums[right] >= pivot) {
                right--;
            }
            nums[left] = nums[right];
            while (left < right && nums[left] <= pivot) {
                left++;
            }
            nums[right] = nums[left];
        }
        
        // 返还pivot点到数组里面
        nums[left] = pivot;
        return left;         
    }
};
```