### 1. prefab 동적 생성하기

1. Assets > Prefabs > Resources 폴더 생성
  - 해당 폴더에 넣은 prefab은 코드를 통해 불러올 수 있다.
2. Script에서 Resource 가져오기

```cs
(GameObject)Instantiate(Resources.Load("prefab명") as GameObject, new Vector3(x좌표, y좌표, x좌표), Quaternion.identity);
```


### 2. 활용하기
#### 1. npc 위에 icon 띄우기

```cs
  float npc_X = npc.transform.position.x;
  float npc_Z = npc.transform.position.z;
  float npc_Y = npc.transform.position.y;
  QuestionMark_icon = (GameObject)Instantiate(Resources.Load("QuestionMark_icon") as GameObject, new Vector3(npc_X, npc_Y + 2f, npc_Z), Quaternion.identity);
  Exclamation_icon = (GameObject)Instantiate(Resources.Load("Exclamation_icon") as GameObject, new Vector3(npc_X, npc_Y + 2f, npc_Z), Quaternion.identity);
	// 바로 생성된다.

	// 숨기기
	QuestionMark_icon.SetActive(false);
	Exclamation_icon.SetActive(false);
  
	// 보이기
	QuestionMark_icon.SetActive(true);
	Exclamation_icon.SetActive(true);
```

#### 2. Rock 주변에 Rock_item 뿌리기

```cs
// 아이템 뿌리기
    void SetItem(GameObject wantObj_rock)
    {
      GameObject instance = (GameObject)Instantiate(Resources.Load("Rock_item") as GameObject, RandomPosition(wantObj_rock), Quaternion.identity);
      // 생성한 아이템 하위로 넣어주기
      instance.transform.parent = wantObj_rock.transform;
      instance.name = "item" + i;
      instance.tag = "Item";
    }

// 랜덤한 위치 설정하기
    Vector3 RandomPosition(GameObject wantObj_rock)
    {
        // 기반이 될 위치
        Vector3 originPosition = wantObj_rock.transform.position;

        float range_X = wantObj_rock.transform.position.x;
        float range_Z = wantObj_rock.transform.position.z;
        float range_Y = wantObj_rock.transform.position.y;

        range_X = UnityEngine.Random.Range((range_X / 4) * -1, range_X / 4);
        range_Z = UnityEngine.Random.Range((range_Z / 4) * -1, range_Z / 4);
        Vector3 randomPosition = new Vector3(range_X, range_Y+2f, range_Z);

        Vector3 planeRandomRange = originPosition + randomPosition;
        return planeRandomRange;
    }
    // 지금은 주변에 나타나 떨어지는 모션을 갖는다.
    // wantObj_rock 에서 튕겨 나오도록 하면 더 멋지지 않을까
```
