### GameObject를 클릭하는 동작

- 원하는 gameobject를 옮기거나 얻거나 할 수 있도록 선택한 object를 판별해보자

```cs
// 레이케스트가 건드린 것을 취득해서 넣어두는 곳
private RaycastHit hit;
public Camera main_camera;

void Update()
{
    // 커서가 사라지는 문제
  Cursor.visible = true;
  Cursor.lockState = CursorLockMode.None;
  
  if (Input.GetMouseButtonDown(0)) // 마우스 왼쪽 클릭
  {
    Ray ray = main_camera.ScreenPointToRay(Input.mousePosition); // 설정한 카메라에서 레이저를 쏴서 물체 탐색
    if (Physics.Raycast(ray, out hit))
    {
      GameObject clickObj = hit.collider.gameObject;
      Debug.Log(clickObj.name);
    }
  }
}
```

- 해당 코드는 선택한 object의 name을 가져온다.
- 플레이어 가까이 있는 object만 클릭할 수 있게 해보자
```cs
if (Input.GetMouseButtonDown(0))
{
  Ray ray = main_camera.ScreenPointToRay(Input.mousePosition);
  if (Physics.Raycast(ray, out hit))
  {
    GameObject clickObj = hit.collider.gameObject;
    //Debug.Log(clickObj.name);
    if(Vector3.Distance(player.transform.position, clickObj.transform.position) < 1f) // object와 player의 거리가 1f 이하일 때
    {
      clickObj.SetActive(false); // 숨김 처리
    }
  }
}
```

+ 가까이 있는 obj 띄워서 돌리기 (y축이 넘어진 obj는 이상해져서,, 일단 빼두자)

