# find-all-stable-binary-arrays
leetcode problem No:3129
class Solution {

    static final int MOD = 1000000007;

    int[][][][] memo;

    public int numberOfStableArrays(int zero, int one, int limit) {

        memo = new int[zero + 1][one + 1][2][limit + 1];

        for (int i = 0; i <= zero; i++)
            for (int j = 0; j <= one; j++)
                for (int k = 0; k < 2; k++)
                    for (int l = 0; l <= limit; l++)
                        memo[i][j][k][l] = -1;

        int startWithZero = dfs(zero - 1, one, 0, 1, limit);
        int startWithOne = dfs(zero, one - 1, 1, 1, limit);

        return (startWithZero + startWithOne) % MOD;
    }

    int dfs(int z, int o, int last, int count, int limit) {

        if (z < 0 || o < 0) return 0;

        if (z == 0 && o == 0) return 1;

        if (memo[z][o][last][count] != -1)
            return memo[z][o][last][count];

        long ans = 0;

        if (z > 0) {
            if (last == 0) {
                if (count < limit)
                    ans += dfs(z - 1, o, 0, count + 1, limit);
            } else {
                ans += dfs(z - 1, o, 0, 1, limit);
            }
        }

        if (o > 0) {
            if (last == 1) {
                if (count < limit)
                    ans += dfs(z, o - 1, 1, count + 1, limit);
            } else {
                ans += dfs(z, o - 1, 1, 1, limit);
            }
        }

        memo[z][o][last][count] = (int)(ans % MOD);
        return memo[z][o][last][count];
    }
}

