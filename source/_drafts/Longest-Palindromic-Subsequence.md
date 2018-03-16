title: Longest Palindromic Subsequence
author: Willy Wang
date: 2018-03-15 21:15:44
tags:
---
```c++
#define lastI size-1
#define lastJ 0
#include <bits/stdc++.h>
class Solution {
public:
    string longestPalindrome(string s) {
        string a;
        int size = s.size();
        int dp[size][size] = {{0}};
        int bt[size][size];
        for( int i = 0; i < size; i++ ) {
            for( int j = 0; j < size; j++ ) {
                bt[i][j] = 2;
            }
        }
        
        for( int l = 0; l < size; l++ ) {
            for( int j = 0; j < size; j++ ) {
                int i = j + l;
                if( i < size ) {
                    if( s[i] == s[j] ) {
                        if( i == j ) {
                            dp[i][j] = 1;
                            bt[i][j] = 3;
                        } else {
                            // dp[i][j] = ( dp[i-1][j] >= dp[i][j+1] )? dp[i-1][j] : dp[i][j+1];
                            // bt[i][j] = ( dp[i-1][j] >= dp[i][j+1] )?     -1     :      1;
                            // dp[i][j] = ( dp[i-1][j+1] + 2 >= dp[i][j] )? dp[i-1][j+1] + 2 : dp[i][j];
                            // bt[i][j] = ( dp[i-1][j+1] + 2 >= dp[i][j] )?        0         : bt[i][j];
                            dp[i][j] = dp[i-1][j+1] + 2;
                            bt[i][j] = 0;
                        }
                    } else {
                        dp[i][j] = ( dp[i-1][j] >= dp[i][j+1] )? dp[i-1][j] : dp[i][j+1];
                        bt[i][j] = ( dp[i-1][j] >= dp[i][j+1] )?     -1     :      1;
                    }
                }
            }
        }//dp
        for( int j = 0; j < size; j++ ) {
            for( int i = 0; i < size; i++ ) {
                // cout << bt[i][j] << '';
                printf("%3d", bt[i][j]);
            }
            cout << '\n';
        }
        
        
        // cout << dp[lastI][lastJ] << '\n';
        int i = lastI, j = lastJ;
        while( bt[i][j] != 2 && bt[i][j] != 3 ) {
            switch( bt[i][j] ) {
                case -1 :
                    i--;
                    break;
                case 0 :
                    a.insert(a.begin(), s[i]);
                    i--;
                    j++;
                    break;
                case 1 :
                    j++;
                    break;
            }
        }
        string tmp = a;
        reverse(tmp.begin(), tmp.end());
        if( bt[i][j] == 3 ) a = tmp + s[i] + a;
        else a = tmp + a;
        return a;
    }
};

string stringToString(string input) {
    assert(input.length() >= 2);
    string result;
    for (int i = 1; i < input.length() -1; i++) {
        char currentChar = input[i];
        if (input[i] == '\\') {
            char nextChar = input[i+1];
            switch (nextChar) {
                case '\"': result.push_back('\"'); break;
                case '/' : result.push_back('/'); break;
                case '\\': result.push_back('\\'); break;
                case 'b' : result.push_back('\b'); break;
                case 'f' : result.push_back('\f'); break;
                case 'r' : result.push_back('\r'); break;
                case 'n' : result.push_back('\n'); break;
                case 't' : result.push_back('\t'); break;
                default: break;
            }
            i++;
        } else {
          result.push_back(currentChar);
        }
    }
    return result;
}

int main() {
    string line;
    while (getline(cin, line)) {
        string s = stringToString(line);
        
        string ret = Solution().longestPalindrome(s);

        string out = (ret);
        cout << out << endl;
    }
    return 0;
}
```