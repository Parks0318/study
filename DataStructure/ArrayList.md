## ArrayList

*알고리즘 분류해보기*
> 아래 add 메서드는 상수시간일까, 선형일까? 
```
public boolean add(T element) {
        if (size >= array.length) {
            @SuppressWarnings("unchecked")
            T[] bigger = (T[]) new Object[array.length * 2];
            System.arraycopy(array, 0, bigger, 0, array.length);
            array = bigger;
        }
        array[size] = element;
        size++;
        return true;
    }
```
- - - 

풀이법

> 간단하게 두 요소만큼의 공간이 있는 배열로 가정
>
> > 첫번째 호출 - 요소a저장
> > 두번째 호출 - 요소a저장
> > 세번째 호출 - 배열의 크기 변경, 요소2개 복사, 요소a저장. (배열의 크기는 4)
> > 네번째 호출 - 요소a저장
> > 5번째 호출 - 배열의 크기 변경, 요소4개 복사, 요소a저장. (배열의 크기는 8)
> > 추가 3번의 호출 - 요소 3개 저장
> > 다음번 호출 - 배열의 크기 변경, 요소8개 복사, 요소a저장. (배열의 크기는 16)
> > ... 반복 ...

* 패턴을 발견할 수 있는데 n번 추가하면 요소 n개를 저장하고 n-2번 복사.
* 총 연산 횟수는 n + n-2, 즉 2n-2 이다.
* 평균 연산 횟수를 구하기 위해서는 합을 n으로 나누어야하고, 결과는 2-2/n
* n이 커지면 두번째 항이 작아지니, add메서드는 상수시간이다. 
* -> 배열의 크기를 조정할 때마다 2배로 늘리도록 정의, 요소를 복사하는 횟수를 제한하여서 이런 분석이 가능. 

- - -

##### LinkedList 맛보기
* 자료구조가 연결되었다 함은 노드(node)라는 객체들이 다른 노드에 대한 참조를 포함한 형태로 저장된 것.
* ListNode에는 어떤 Object에 대한 참조(= data)와 리스트에서 다음 노드에 대한 참조(= next)를 가진다.
