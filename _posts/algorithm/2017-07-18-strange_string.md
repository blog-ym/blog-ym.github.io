# 이상한 문자 만들기



```
def toWeirdCase(s):
    s = s.lower()
    a = s.split()
    c=[]

    for i in range(len(a)):
        b=[]
        for j in range(len(a[i])):

            if j % 2 == 0:
                b.append(a[i][j].upper())
            else:
                b.append(a[i][j])
                print('{} if 통과후{}'.format(j,b))

            print('{} for 통과후 {}'.format(i,b))
            result = ' '.join(b)
        c.append(result)

    return ','.join(c).replace(' ','').replace(',', ' ')
```