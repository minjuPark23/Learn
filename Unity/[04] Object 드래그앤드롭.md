### GameObject 드래그 앤 드롭
 
*>> 3차원은 매우 어렵다. 지금은 고정된 Camera내에서 일정한 거리에만 둘 수 있다*
- 특정한 상황에서 특정 obj만 옮길 수 있도록 prefab에 Script를 넣어뒀다.
- 돌이켜 생각해보면 특정한 상황을 알려주는 변수랑 tag로 찾아도 될 것 같다.

```cs
private void OnMouseDown() // 마우스 클릭
{
  Debug.Log("터치");
  //screenSpace = Camera.main.WorldToScreenPoint(transform.position);
  PlaySound("PICK");
  //offset = transform.position - Camera.main.ScreenToViewportPoint(new Vector3(Input.mousePosition.x, Input.mousePosition.y, screenSpace.z));
}

private void OnMouseDrag()
{
  // 이해가 필요한 부분..
  //Vector3 curScreenSpace = new Vector3(Input.mousePosition.x, Input.mousePosition.y, screenSpace.z);
  //Vector3 curPosition = Camera.main.ScreenToWorldPoint(curScreenSpace) + offset;
  
  // 현재는 Z축을 카메라에서 1.9f에 뒀다
  Vector3 curPosition = new Vector3(Input.mousePosition.x, Input.mousePosition.y, 1.9f);
  
  // prefab에 넣어놨기 때문에 GameObject에 대한 transform.position을 뜻한다.
  transform.position = temp.ScreenToWorldPoint(curPosition);
  Debug.Log("드래그");
}

private void OnMouseUp()
{
  // 떨어지는 것은 rigibody를 통해 중력을 잡도록 하였다.
  Debug.Log("드롭");
  PlaySound("PUT");
}
```

### 드래그 앤 드롭 시 소리 넣기

```cs
// AudioClip을 하나씩 연결해줬는데 배열로 넣어도 될 것 같다.
// public AudioClip[] audio;
public AudioClip audioPick;
public AudioClip audioPut;

private void Awake()
{
  // 시작 전 prefab에 AudioSource 추가하기
  audioSource = gameObject.AddComponent<AudioSource>();
}

void PlaySound(string action)
{
  switch (action)
  {
    case "PICK":
      audioSource.clip = audioPick;
      break;
    case "PUT":
      audioSource.clip = audioPut; 
      break;
  }
  // 꼭 할 때마다 play를 넣어줘야 하는게 이상하다고 생각함..
  audioSource.Play();
}
```
