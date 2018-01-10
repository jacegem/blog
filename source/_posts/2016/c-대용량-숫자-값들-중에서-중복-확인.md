title: '[c#] 대용량 숫자 값들 중에서 중복 확인'
tags: [ c#, array, duplcate, 중복 ]
date: 2016-01-12 15:56:06

---
[c#] 대용량 숫자 값들 중에서 중복 확인

1. 총 숫자 카운트 확인
2. 배열 생성
3. 배열에 할당
4. Array.Sort
5. 이전값과 비교

위와 같은 순서로 진행합니다.



총 숫자 카운트를 확인합니다. 파일에 저장된 데이터를 읽습니다. 한줄에 하나씩의 데이터를 읽어오기 때문에, 줄 수를 카운트 합니다.

```cs
string path = @"c:\test\code\"; 
string[] filePaths = Directory.GetFiles(path);

int lineCnt = 0;
foreach (string file in filePaths){
    string[] lines = System.IO.File.ReadAllLines(file);
    lineCnt += lines.Length;
}
```

카운트 크기의 배열을 생성합니다. 

```cs
long[] codes = new long[lineCnt];
```

다시 파일을 읽으면서 원하는 정보를 추출하여 할당합니다.
```cs
int rowIdx = 0;
long val = 0;
long[] codes = new long[lineCnt];
foreach (string file in filePaths) {
    string[] lines = System.IO.File.ReadAllLines(file);
    foreach (string line in lines) {
    	string[] infos = line.Split('\t');
        string firstVal = infos[0];
        codes[rowIdx++] =  long.Parse(code);
    }
}
    
```

값을 할당한 배열을 정렬합니다. 

```cs
Array.Sort(codes);

```
          
정렬된 배열에서 중복값을 확인합니다. 이전값과 같은지 비교합니다.

```cs
long last = -1;
for (int i = 0; i < rowIdx; i++) { 
	cd = codes[i];
	if (last == cd) {
		Console.WriteLine("중복:"+cd);
        Console.ReadLine();
    }
  	else {
    	last = cd;
    }
}
```

## 참고 
* [Sorting Arrays [C#]](http://www.csharp-examples.net/sort-array/)
* [c# Stopwatch 사용 ( 시간체크, 동작시간 )](http://infodbbase.tistory.com/112)

