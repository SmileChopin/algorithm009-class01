学习笔记
��Trie���ݽṹ���word search����ʱ�临�Ӷȷ���
1. ʵ��Trie
struct TrieNode {
    bool isWord;
    //  ���ò�ʹ��ָ��
    unordered_map<char, TrieNode*> next;
};

class Trie {
public:
    /** Initialize your data structure here. */
    Trie() {
        root = new TrieNode();
    }

    /** Inserts a word into the trie. */
    void insert(string word) {
        // ����word���ҵ�������µĽڵ㣬�����һ����ĸ���ýڵ�isWord = true
        TrieNode* p = root;
        for (char c : word) {
            if (p->next[c] == nullptr) {
                p->next[c] = new TrieNode();
            }
            p = p->next[c];
        }
        p->isWord = true;
    }

    /** Returns if the word is in the trie. */
    bool search(string word) {
        // ����word�������ĸ��Ӧ���ǿսڵ�򵽸ýڵ�Ϊֹ������Ч���ʣ�����false
        TrieNode* p = root;
        for (char c : word) {
            if (p->next[c] == nullptr) {
                return false;
            }
            p = p->next[c];
        }
        return p->isWord ? true : false;
    }

    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        // ����prefix�������ĸ��Ӧ���ǿսڵ㣬����false
        TrieNode* p = root;
        for (char c : prefix) {
            if (p->next[c] == nullptr) {
                return false;
            }
            p = p->next[c];
        }

        return true;
    }

private:
    TrieNode* root;

};
2.DFS������ĸ����trie�жϵ����Ƿ���Ч����Ч�ͱ��浽���ս���У�Ȼ�����DFS
vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        // ����trie
        for (string w : words) {
            trie.insert(w);
        }

        // ������ĸ����trie�ж�prefix�Ƿ���Ч�������Ч���ټ���DFS
        // ��trie�ж�word�Ƿ���Ч����Ч�ͱ��浽���ս���У�����DFS
        int m = board.size();
        int n = board[0].size();
        string s;
        vector<string> ans;
        unordered_set<string> unique;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                helper(board, i, j, unique, s);
            }
        }

        for (string w : unique) {
            ans.push_back(w);
        }

        return ans;
    }
3.ʱ�临�Ӷȷ���
����N������ĸ, ʱ�临�Ӷ�O(N)
ÿ������ĸDFS, ʱ�临�Ӷ�O(N)
ÿ��string����trie�ж��Ƿ���Ч, ʱ�临�Ӷ�O(M), M������ƽ������
�ܵ�ʱ�临�Ӷ�Ϊʱ�临�Ӷ�O(N*N*2M)
void helper(vector<vector<char>>& board, int row, int col,
                    unordered_set<string>& unique, string s) {
    int m = board.size();
    int n = board[0].size();
    if (row < 0 || row >= m || col < 0 || col >= n || board[row][col] == '#') {
        return;
    }

    int dx[4] = {0, 0, -1, 1};
    int dy[4] = {-1, 1, 0, 0};
    s += board[row][col];
    if (trie.startsWith(s)) {
        if (trie.search(s)) {
            unique.insert(s);
        }
        char c = board[row][col];
        board[row][col] = '#';
        for (int i = 0; i < 4; i++) {
            helper(board, row + dy[i], col + dx[i], unique, s);
        }
        board[row][col] = c;
    }
}
4.�Ľ����������ضԵ��ʱ���ÿ����ĸ������trieȥ�ж�string����Ч��
���ȸĽ�Trie����bool isWord�ĳ�string word����insert�����У�����������ʵ�β�ڵ㣬�����ʱ��浽word�С�
��Σ��ڱ�����ĸ��֮ǰ�����ݵ��ʱ���trie
DFS�����У���һ��TrieNodeָ����ٵ�ǰ��ĸ��Ӧ��trie�ڵ�λ�ã�ֱ���ж�ָ��ָ��Ľڵ�word�Ƿ���Ч�������� ������ÿ�δ�ͷ����trie�ڵ㡣
����N������ĸ, ʱ�临�Ӷ�O(N)
ÿ������ĸDFS, ʱ�临�Ӷ�O(N)
ÿ������string����ĸ�ж϶�Ӧtrie�нڵ��Ƿ񹹳���Ч���ʣ�ʱ�临�Ӷ�O(1)
��ʱ�临�Ӷ�O(N * N)
trie insert����
TrieNode* insert(vector<string>& words) {
    TrieNode* root = new TrieNode();
    // ��trie�б���ÿ������
    for (string word : words) {
        TrieNode* p = root;
        // ����ÿ����ĸ���ҵ�������µĽڵ㣬�����һ����ĸ���ڽڵ��б���word
        for (char c : word) {
            if (p->next[c] == nullptr) {
                p->next[c] = new TrieNode();
            }
            p = p->next[c];
        }
        p->word = word;
    }
    return root;
}
dfs helper
// TrieNode* p����׷�ٵ�ǰ��ĸ��Ӧtrie�еĽڵ�
void helper(vector<vector<char>>& board, int row, int col, vector<string>& ans, TrieNode* p) {
    int m = board.size();
    int n = board[0].size();
    int dx[4] = {0, 0, -1, 1};
    int dy[4] = {-1, 1, 0, 0};

    if (row < 0 || row >= m || col < 0 || col >= n || board[row][col] == '#') {
        return;
    }

    char c = board[row][col];
    if (p->next[c] == nullptr) {
        return;
    }

    p = p->next[c];
    if (p->word.size() > 0) {
        ans.push_back(p->word);
        // ��ֹ�ظ���
        p->word = "";
    }
    board[row][col] = '#';
    for (int i = 0; i < 4; i++) {
        helper(board, row + dy[i], col + dx[i], ans, p);
    }
    board[row][col] = c;

}
������
vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
    // ����trie
    TrieNode* root = insert(words);

    // ������ĸ����trie�ж�prefix�Ƿ���Ч�������Ч���ټ���DFS
    // ��trie�ж�word�Ƿ���Ч����Ч�ͱ��浽���ս���У�����DFS
    // ʱ�临�Ӷȷ�����1������N������ĸO(N) 2��ÿ������ĸDFS, O(N)
    // 3��ÿ��string�ж�word.size(), O(1)
    // �ܵ�ʱ�临�Ӷ�ΪO(N*N)
    int m = board.size();
    int n = board[0].size();
    vector<string> ans;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            helper(board, i, j, ans, root);
        }
    }

    return ans;
}
���鼯��ʵ�ַ�ʽ��·�����룬��Ȩ�غϲ�
- ���ڲ��鼯������˹���㷨���кܺõĽ���
// class Union�г�Ա����
int Find(int p) {
    while (p != id[p]) {
        id[p] = id[id[p]]; // ·�����룬��ǰ�ڵ�ĸ��ڵ�ָ���游�ڵ�
        p = id[p];
    }
    return p;
}

bool connected(int p, int q) {
    return Find(p) == Find(q);
}

// Ȩ��С�ļ��ϱ�Ȩ�ش�ļ����̲�
void Union(int p, int q) {
    int rootP = Find(p);
    int rootQ = Find(q);
    if (rootP == rootQ) return;
    if (size[rootP] < size[rootQ]) {
        id[rootP] = rootQ;
        size[rootQ] += size[rootP];
    }
    else {
        id[rootQ] = rootP;
        size[rootP] += size[rootQ];
    }
}
���鼯��·��ѹ�����и����׵ķ�ʽ��

int Find(int p) {
    int root = p;
    while (root != id[root]) {
        root = id[root];
    }

    // �����нڵ㶼ָ����ڵ�
    while (p != root) {
        int tmp = id[p]; // ���游�ڵ�
        id[p] = root;    // ���¸��ڵ�
        p = tmp;         // ׼��������һ��
    }

    return root;
}
