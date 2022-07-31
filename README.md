#写在之前
项目中用到的函数简介：https://github.com/EveandBob/Introduction-to-some-functions-in-elliptic-curves-not-projects-

# 项目名称
通过e和r，s来伪造签名

# 实验内容
伪造私钥的步骤如下：
![Screenshot 2022-07-31 142900](https://user-images.githubusercontent.com/104854836/182013243-cff24455-49fb-4ff2-8e98-1403ac1fb3b4.jpg)
这里面只需要自己随机选取u，v就可以按照步骤来伪造签名

题目中的需要指导中本聪的公钥，签名和计算的hash

在google中查询找到了一组

P=[26877259512020005462763638353364532382639391845761963173968516804546337027093, 48566944205781153898153509065115980357578581414964392335433501542694784316391]
r,s=41159732757593917641705955129814776632782548295209210156195240041086117167123, 57859546964026281203981084782644312411948733933855404654835874846733002636486

# 部分代码
1.伪造签名
```python
    def forge_sign():
        u = random.randrange(1, n)
        v = random.randrange(1, n)
        temp1 = calculate_np(Gx, Gy, u, a, b, p)
        temp2 = calculate_np(P[0], P[1], v, a, b, p)
        R = x, y = calculate_p_q(temp1[0], temp1[1], temp2[0], temp2[1], a, b, p)
        f_r = x % n
        f_e = (f_r * u * get_inverse(v, n)) % n
        f_s = (f_r * get_inverse(v, n)) % n
        return f_r, f_s, f_e

    r,s,e=forge_sign()
```
2.对伪造的签名进行验证
```python
    def verify(r,s,e):
        w = get_inverse(s, n)
        temp1 = calculate_np(Gx, Gy, e * w, a, b, p)
        temp2 = calculate_np(P[0], P[1], r * w, a, b, p)
        R, S = calculate_p_q(temp1[0], temp1[1], temp2[0], temp2[1], a, b, p)
        if (r == R):
            return True
        else:
            return False
```
# 实现结果
![Screenshot 2022-07-31 143320](https://user-images.githubusercontent.com/104854836/182013376-2da73c63-6f06-499c-9384-834e7a45a7a5.jpg)

