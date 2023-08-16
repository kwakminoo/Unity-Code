# Unity-Code


플레이어 이동
------
<pre>
<code>
private float speed;
private float turnspeed;
private float horizontalInput;
private float forwardInput;

void Update()
{
  //키보드 w,a,s,d로 움직인다 라는 것을 불러옴
  horizontalInput = Input.GetAxis("Horizontal");
  forwardInput = Input.GetAxis("Vertical");

  //Vecrtor3.forward는 앞 뒤로, Vector3.Up은 양 옆으로 회전
  transform.Translate(Vector3.forward * Time.deltaTime * speed * forwardInput);
  //양 옆을 이동하는 속도를 다르게 할려면 앞뒤 이동 속도 변수와 양옆 이동 속도 변수를 따로 생성해야함
  transfrom.Translate(Vector3.right * horizontalInput * Time.deltaTime * speed);
  transform.Rotate(Vector3.up, turnspeed * horizontalInput * Time.deltaTime);
}
</code>
</pre>

플레이어 카메라 시점 설정
------------

<pre>
<code>
public GameObject player;
private Vector3 offset = new Vector3(x, y, z);

  //Late를 추가해야 시점이 부드러워짐
  void LateUpdate()
  {
    transform.position = player.transfrom.position + offset;
  }
</code>
</pre>

물체를 회전
----------

<pre>
<code>
void Update()
  {
    //첫 번째 방법
    transform.rotation = new Vector3(x, y, z);
    //두 번째 방법
    transfrom.rotation = Quaternion.Euler(x, y, z);
    //세 번째 방법
    transfrom.eulerAngles = new Vector3(x, y, z);
  }
</code>
</pre>

위치 이탈 방지
-----------
<pre>
<code>
void Update()
  {
    if (transform.position 축 < 위치)
      {
       //위치를 고정하고 싶으면 transform.position.축 대신 고정하고 싶은 위치를 적으면 그 위치 밖으로 못감
       transfrom.position = new Vector3(transfrom.position.x,  transfrom.position.y, transfrom.position.z);
      }
  }
</code>  
</pre>

  
