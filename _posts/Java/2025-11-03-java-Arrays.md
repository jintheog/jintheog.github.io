---
title: "Variable"
date: 2025-11-03 00:00:00 +0900
categories: [Java]
tags: [OOP, Java]
---

{% raw %}

```Java
import java.util.Arrays;
public class ArraysUtility {
    public static void main(String[] args) {
        int[] numbers = {5, 2, 8, 1, 9};

        // 1. toString() - 배열을 문자열로
        System.out.println("배열: " + Arrays.toString(numbers));

        // 2. sort() - 정렬
        Arrays.sort(numbers);
        System.out.println("정렬 후: " + Arrays.toString(numbers));

        // 3. binarySearch() - 이진 탐색 (정렬된 배열에서)
        int index = Arrays.binarySearch(numbers, 5);
        System.out.println("5의 인덱스: " + index);

        // 4. fill() - 배열 채우기
        int[] filled = new int[5];
        Arrays.fill(filled, 10);
        System.out.println("채워진 배열: " + Arrays.toString(filled));

        // 5. copyOf() - 배열 복사
        int[] original = {1, 2, 3, 4, 5};
        int[] copied = Arrays.copyOf(original, 3);
        System.out.println("복사된 배열: " + Arrays.toString(copied));

        // 6. copyOfRange() - 범위 복사
        int[] ranged = Arrays.copyOfRange(original, 1, 4);
        System.out.println("범위 복사: " + Arrays.toString(ranged));

        // 7. equals() - 배열 비교
        int[] arr1 = {1, 2, 3};
        int[] arr2 = {1, 2, 3};
        System.out.println("배열 같음: " + Arrays.equals(arr1, arr2));

        // 8. deepToString() - 다차원 배열 출력
        int[][] matrix = {{1, 2}, {3, 4}};
        System.out.println("2차원 배열: " + Arrays.deepToString(matrix));

        // 9. deepEquals() - 다차원 배열 비교
        int[][] m1 = {{1, 2}, {3, 4}};
        int[][] m2 = {{1, 2}, {3, 4}};
        System.out.println("2차원 배열 같음: " + Arrays.deepEquals(m1, m2));
    }
}

```

## 배열 복사 주의 사항

```Java
public class ArrayCopy {
    public static void main(String[] args) {
        int[] original = {1, 2, 3, 4, 5};

        // 얕은 복사 (참조만 복사) - 위험!
        int[] shallow = original;
        shallow[0] = 100;
        System.out.println("원본: " + Arrays.toString(original));  // [100, 2, 3, 4, 5]

        // 깊은 복사 방법 1: Arrays.copyOf()
        int[] deep1 = Arrays.copyOf(original, original.length);
        deep1[0] = 200;
        System.out.println("원본: " + Arrays.toString(original));  // [100, 2, 3, 4, 5]
        System.out.println("복사본: " + Arrays.toString(deep1));    // [200, 2, 3, 4, 5]

        // 깊은 복사 방법 2: System.arraycopy()
        int[] deep2 = new int[original.length];
        System.arraycopy(original, 0, deep2, 0, original.length);

        // 깊은 복사 방법 3: clone()
        int[] deep3 = original.clone();
    }
}

```

{% endraw %}
