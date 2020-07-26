���������ѧϰ�ܽ�
���ֲ���
���ֲ����Ƿ���˼���һ�־������֣�������һ������������ʹ�ö��ֲ��ң�ÿһ�β����������м�ֵ��Ȼ�󽫲������⻮�ֳ����������ֲ��ҵ������⡣���ֲ��ҵ��ѵ����ڲ��������ʱ��ı߽紦��ͷ���ֵ������LeetCode���ҵ�������ģ�棬�����Ŵ��Լ��ĽǶ�ȥ��Ⲣ���䡣

ģ��1:
// ��ʼֵleft = 0, right = nums.size() - 1
int binarySearch(vector<int>& nums, int target, int left, int right){
  if (left > right) {
    return -1;
  }

  int mid = left + (right - left) / 2;
  if (nums[mid] == target) {
    return mid;
  }
  else if (nums[mid] < target) {
    // [mid + 1, right]
    return binarySearch(nums, target, mid + 1, right);
  }
  else {
    // [left, mid - 1]
    return binarySearch(nums, target, left, mid - 1);
  }
}
int mid = left + (right - left) / 2�Ƿ�ֹ(left + right)�������������������ұ�����ı�ʾ��ʽ�����������ʾΪ[L, R], ����������󣬲�������������ֱ�Ϊ[L, mid - 1], [mid + 1, R]�����ҹ��̿��ԽӴ������ұ߽�ֵ�����統���䳤��Ϊ2ʱ����nums[mid] < target�������������Ϊ[R, R]����������������Ϊ[L, L]������ܹ��ҵ��������ͻ᷵��L����R�������ٽ�һ���������������Ϊ[L, L - 1]��[R + 1, R]����ʱ����ݹ������������-1��

ģ��2:
// ��ʼֵleft = 0, right = nums.size()
int binarySearch(vector<int>& nums, int target, int left, int right){
  if (left == nums.size()) return -1;
  if (left == right) {
    return (nums[left] == target ? left : -1);
  }

  int mid = left + (right - left) / 2;
  if(nums[mid] == target){ return mid; }
  else if(nums[mid] < target) {
    // [mid + 1, right)
    return binarySearch(nums, target, mid + 1, right);
  }
  else {
    // [left, mid)
    return binarySearch(nums, target, left, mid);
  }
}
ģ��2�����������ҿ��ĵı�ʾ��ʽ�����������ʾΪ[L, R)���������������������ֱ�Ϊ[L, mid), [mid + 1, R)�����ҹ���ֻ�ܽӴ�����߽�, ����������ĳ���Ϊ2����nums[mid] != target�������������Ϊ[R, R)����[L, L)����ʱ�Ѿ�����ݹ����������������Ҫ�ж�nums[R] == target����nums[L] == target��

ģ��3
// ��ʼֵleft = 0, right = nums.size() - 1
int binarySearch(vector<int>& nums, int target){
    if (left + 1 == right) {
      if (nums[left] == target) return left;
      else if (nums[right] == target) return right;
      else return -1;
    }

    int mid = left + (right - left) / 2;
    if (nums[mid] == target) return mid;
    else if (nums[mid] < target) {
      return binarySearch(nums, taget, mid, right);
    }
    else {
      return binarySearch(nums, taget, left, mid);
    }
}
ģ��3����������ұ�����ı�ʾ��ʽ�����������ʾΪ[L, R], �������������������ֱ�Ϊ[L, mid], [mid, R]�����ҹ����޷��Ӵ������ұ߽磬�����䳤��Ϊ2����ʱ�Ѿ�����ݹ����������������Ҫ�ж�nums[L] == target����nums[R] == target��

�ܽᣬ�Ӷ��ֲ��ҹ��̶����ұ߽�ķ��ʿ��Էֳ�����ģ�棬��Ӧ��ͬ�Ĵ������󡣶���ģ��2��ģ��3ҪС�Ĵ���߽���ʡ�

������ݹ�
��n�ʺ�����ѧϰ�ݹ�����ݣ��ݹ麯������4��Ҫ�أ�

��������
�����߼�
������һ����㣬���������
���ݽ׶Σ������ǰ״̬��Ϊ��һ�μ�����׼��
��n�ʺ�����ĵݹ�⣺

class Solution {
public:
    // a��ʾ�г�ͻ��b��ʾ�Խ��߳�ͻ��c��ʾ���Խ��߳�ͻ
    void put(int row, int n, vector<vector<string>>& ans, vector<string>& solve,
            int a, int b, int c) {
        if (row == n) {
            // ����ⷨ
            ans.push_back(solve);
            return;
        }

        // �ڵ�ǰ�з���Queen, �ж��ֿ���
        // ȡ����ȡ��nλ��bit = 1��ʾ��Чλ
        int valid = ~(a | b | c) & ((1 << n) - 1);
        while (valid) {
            // ֻ���������Чλ
            int leftmost = valid & (-valid);
            int i = log2(leftmost);
            string s(n, '.');
            s[n - 1 - i] = 'Q';
            solve.push_back(s);
            // ȥ��һ�з���Queen
            put(row + 1, n, ans, solve, a | leftmost, (b | leftmost) << 1,
                (c | leftmost) >> 1);

            // ���ݣ������ǰ�⣬׼����һ��
            solve.pop_back();
            // ��������Чλ, ׼��������һ��
            valid = valid & (valid - 1);
        }
    }

    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> ans;
        vector<string> solve;
        put(0, n, ans, solve, 0, 0, 0);
        return ans;
    }
};
����vector<string>& solve���ڱ����м�״̬������vector<vector<string>>& ans���ڱ������ս⡣�ݹ���һ��һ������ģ�ÿһ�㶼Ҫ�ݴ��м�״̬��ֱ���ݹ鴥�׷��أ�����Ĵ��׾�������ݹ������������ͼ��һ���ݹ�ִ�й��̵�ʾ��ͼ��



�ݹ��1��ʼ��Ȼ��2���ٵ�3��������룬ÿһ�㶼�����ж���м�״̬������״̬2��6��8����3��4��5����ĳһ����м�״̬��������ݹ�����ʱ����Ҫ�ѵ�ǰ�������м�״̬���浽���ս�ans���n�ʺ������оͶ�Ӧ��ÿһ�����Queen�ķ�����Ȼ����㽫�м�״̬���������1��2��3��һ���⣬Ȼ������м�״̬3�����ݵ��м�״̬2��Ȼ���ٵݹ鵽�м�״̬4������������������ͱ��浽���ս⡣�������м�״̬��������ɺ������ݹ���м�״̬������ա�������𰸶����������ս�ans��ڴ����������м�״̬���������ò�������ʽvector<string>& solve��㴫����ȥ�ģ������Ҫ�������Լ�����м�״̬�������ĺô��ǽ�ʡ�ڴ棬��������������ʱ������������ô�ֵ����vector<string> solve�ķ�ʽ��������ݹ����ʱ�������߲����Լ�����м�״̬����Ҫ�ڱ����ڲ�ͬ�м�״̬֮������滻���Һ������������˶������ʱ���档
学习笔记
