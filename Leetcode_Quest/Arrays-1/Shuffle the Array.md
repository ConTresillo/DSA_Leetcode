![[Pasted image 20260103205721.png]]

class Solution {

    public int[] shuffle(int[] nums, int n) {

        int[] newArr = new int[2*n];

        for(int i=0; i<n; i++){

            newArr[2*i] = nums[i];

        }

        for(int i=n; i<2*n; i++){

            newArr[(2*i)%(2*n) + 1] = nums[i];

        }

        return newArr;

    }

}

