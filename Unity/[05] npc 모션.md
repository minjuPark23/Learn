### 걷는 npc
- 단순히 걷는 npc를 만들려고 한다..
- plane 범위를 벗어나면 y축을 반대편 각도로 랜덤하게 돌려서 주변을 걸어다니는 npc를 만들 수 있을 것 같다,
- 여기서는 그냥 걷는 의미로 Time.deltaTime을 위해 간단한 코드

```cs
// 특정 시점까지만 움직이기 위해서
static bool wolfStop = false;

void Update()
{
  if (!wolfStop)
  {
    transform.Translate(Vector3.forward * 4.5f * Time.deltaTime, Space.Self);
  }
}

// 특정 시점에 해당 함수를 부른다.
public static void WolfStop()
{
  wolfStop = true;
}
```

여기서 Time.deltaTime이란?
> 전 프레임이 완료되기까지 걸린 시간을 뜻한다
- 쓴 이유는 컴퓨터 마다의 격차를 줄이기 위해서 사용된다.

```
Update가 실행될 때마다 1 프레임 당 이동거리를 계산하여 실행한다. 그렇다면 컴퓨터의 사양에 맞게 1초에 실행되는 횟수가 달라도 똑같은 결과를 낼 수 있다,

속도 = 거리 / 시간
이동 속도 = 캐릭터의 이동거리 / 초 <- 양변에 Time.deltaTime(1 프레임 당 실행 시간)을 곱해준다.
이동 속도 * 1 프레임 당 실행 시간 = 1 프레임 당 실행 시간 * 캐릭터 이동거리 / 초

속도 * 시간 = 거리
이동 속도 * 1 프레임 당 실행 시간  = 1 프레임 당 이동 거리

즉, 1 프레임 당 이동 거리 = 1 프레임 당 실행 시간 * 캐릭터 이동 속도
=> 성능이 안좋은 컴퓨터는 프레임 실행 시간이 길어져도 같이 이동 거리가 늘어나게 된다.
  그래서 성능 좋은 컴퓨터와의 격차를 줄일 수 있는 것이다.
```

[참고]
1. [Time.deltaTime](https://inyongs.tistory.com/18)
