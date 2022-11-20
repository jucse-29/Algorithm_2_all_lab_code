# Algorithm_2_all_lab_code



#include <bits/stdc++.h>
using namespace std;
const int N = 1e3;
int A[N];
int B[N];
int Lazy[N];
int rn;
int query(int l, int r)
{
       int mi = 9990;
       int lb = (l - 1) / rn;
       lb++;
       int rb = (r - 1) / rn;
       rb++;

       for (int i = lb + 1; i <= rb - 1; i++)
       {
              mi = min(mi, B[i] + Lazy[i]);
       }
       if (Lazy[lb] == 1)
       {
              for (int i = rn * (lb - 1) + 1; i <= lb * rn; i++)
              {
                     A[i] += Lazy[lb];
              }
              Lazy[lb] = 0;
              B[lb] = 99999;
              for (int i = rn * (lb - 1) + 1; i <= (lb)*rn; i++)
              {
                     B[lb] = min(B[lb], A[i]);
              }
       }
       for (int i = l; i <= min(r, lb * rn); i++)
       {
              mi = min(mi, A[i]);
       }
       if (Lazy[rb])
       {
              for (int i = (rb - 1) * rn + 1; i <= (rb)*rn; i++)
              {
                     A[i] += Lazy[rb];
              }
              Lazy[rb] = 0;
              B[rb] = 999999;
              for (int i = (rb - 1) * rn + 1; i <= (rb)*rn; i++)
              {
                     B[rb] = min(B[rb], A[i]);
              }
       }
       for (int i = max(l + 1, (rb - 1) * rn + 1); i <= r; i++)
       {
              mi = min(mi, A[i]);
       }
       return mi;
}
void point_update(int idx, int value)
{
       int b = (idx - 1) / rn + 1;
       if (Lazy[b])
       {

              for (int i = (b - 1) * rn + 1; i <= (b)*rn; i++)
              {
                     A[i] = A[i] + Lazy[b];
                     // B[b]+=A[i];
              }

              A[idx] = value;
              B[b] = 999999;
              for (int i = (b - 1) * rn + 1; i <= (b)*rn; i++)
              {
                     B[b] = min(B[b], A[i]);
              }
              Lazy[b] = 0;
       }
       else
       {
              B[b] = 999999;
              A[idx] = value;
              for (int i = (b - 1) * rn + 1; i <= (b)*rn; i++)
              {
                     B[b] = min(B[b], A[i]);
              }
       }
}
void range_update(int l, int r, int value)
{
       int lb = (l - 1) / rn + 1;
       int rb = (r - 1) / rn + 1;
       for (int i = lb + 1; i <= rb - 1; i++)
       {
              Lazy[i] += value;
       }
       if (Lazy[lb])
       {
              for (int i = rn * (lb - 1) + 1; i <= (lb)*rn; i++)
              {
                     A[i] += Lazy[lb];
              }
              Lazy[lb] = 0;
       }
       for (int i = l; i <= min(r, (lb)*rn); i++)
       {
              A[i] += value;
       }
       B[lb] = 99999;
       for (int i = rn * (lb - 1) + 1; i <= (lb)*rn; i++)
       {
              B[lb] = min(B[lb], A[i]);
       }
       if (Lazy[rb])
       {
              for (int i = rn * (rb - 1) + 1; i <= (rb)*rn; i++)
              {
                     A[i] += Lazy[rb];
              }
              Lazy[rb] = 0;
       }
       B[rb] = 9999999;
       for (int i = max((rb - 1) * rn + 1, l); i <= r; i++)
       {
              A[i] += value;
       }
       for (int i = rn * (rb - 1) + 1; i <= (rb)*rn; i++)
       {
              B[rb] = min(B[rb], A[i]);
       }
}
void pre(int n)
{
       rn = sqrt(n);
       int bsize = n / rn;
       for (int j = 1; j <= bsize; j++)
       {
              for (int i = (j - 1) * rn + 1; i <= j * rn; i++)
              {
                     B[j] = min(B[j], A[i]);
              }
       }
}
int main()
{
       int n;
       cin >> n;
       for (int i = 1; i <= n; i++)
       {
              cin >> A[i];
       }

       pre(n);
       int q;
       cin >> q;
       while (q--)
       {
              int a;
              cin >> a;
              if (a == 1)
              {
                     int c, d;
                     cin >> c >> d;
                     cout << query(c, d) << endl;
              }
              else if (a == 2)
              {
                     int c, d;
                     cin >> c >> d;
                     point_update(c, d);
              }
              else
              {
                     int c, d, e;
                     cin >> c >> d >> e;
                     range_update(c, d, e);
              }
       }
}
