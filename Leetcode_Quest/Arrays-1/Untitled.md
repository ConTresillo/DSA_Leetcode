![[Pasted image 20260103212610.png]]

class Solution {

    public int findMaxConsecutiveOnes(int[] nums) {

        int cnt = 0, max = 0;

        int n = nums.length;

        for(int i=0; i<n; i++){

            if(nums[i]==1){

                cnt++;

            }

            else{

                max = Math.max(max,cnt);

                cnt = 0;

            }

        }

        max = Math.max(max,cnt);

        return max;

    }

}