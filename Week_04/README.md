学习笔记
Ѱ�Ұ����������м�����ĵط�
�������
�������������ݣ��м�����ĵط���ָĳ��������Ӧ��ֵ���ں�һ��ֵ����nums[i] > nums[i+1]��

[3, 4, 5, 6, 7, 0, 1, 2], 7���ں����0��7��Ӧ������Ϊ4�������м�����ĵط�����Ϊ4
[6, 7, 0, 1, 2, 4, 5], �м�����ĵط�����Ϊ1
[7, 0, 1, 2, 4, 5, 6], �м�����ĵط�����Ϊ0
[0, 1, 2, 4, 5, 6], ��������ģ����Է���-1
˼·
ʹ�ö��ַ��������ݣ�������ѭ������Ϊleft < right, ѭ������ʱ������������һ���߽�û�д�������ʽΪ[left, left]����[right, right], ���left����rightС��nums.size() - 1������Ҫ�ж�nums[left] > nums[left + 1]��nums[right] > nums[right + 1]�������

����mid = left + (right - left)/2����Ϊѭ������left < right������mid + 1����Խ�磬�����nums[mid] > nums[mid + 1]��˵���ҵ�������ط���Ӧ������������mid��

���û���ҵ���ͨ��nums[mid]��nums[left]��ϵ�жϣ��������һ��������ģ������һ�����������[left, mid - 1]����[mid + 1, right]��

��һ��������ֶ�[left, mid - 1]��[mid + 1, right]��Ѱ������㣬������������������պ�������ֶε�mid��������ʱ����ֱ�ӷ��ء�������������������ұ߽�[left, left]��[right, right]�������������Ҫ��ѭ��������⡣�����δ�ҵ�����㣬˵���������鶼������ģ�Ӧ�÷���-1��

����
class Solution {
public:
    int search(vector<int>& nums) {
        int left = 0;
        int right = nums.size() - 1;

        while (left < right) {
            int mid = left + (right - left) / 2;

            if (nums[mid] > nums[mid + 1]) {
                return mid;
            }

            if (nums[mid] >= nums[left]) {
                left = mid + 1;
            }
            else {
                right = mid - 1;
            }
        }

        if (left != nums.size() - 1 && nums[left] > nums[left+1]) {
            // cout << nums[left] << ',' << nums[left+1] << endl;
            return left;
        }

        return -1;
    }
};
