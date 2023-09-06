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

발사체 날리기
-------------
<pre>
<code>
public float speed = 발사체 속도;

void Update()
  {
    trnasform.Translate(Vector3.forward * Time.deltaTime * speed);
  }
</code>
</pre>

키를 눌러 발살체 발사
---------------
<pre>
<code>
void Update()
  {
    //GetKey, GetKeyUp, GetKeyDown 등을 이용함, KeyCode.플레이어가 누를 키 이름을 입력
    if (Input.GetKeyDown(KeyCode.Space))
      {
        Instantiate(프로젝트프리펩, transform.position, 프로젝트프리펨.transfrom.rotation);
      }
  }
</code>
</pre>


화면을 벗어난 오브젝트 제거
-
<pre>
<code>
private float 변수 = 위치 수;

void Update()
  {
    if (transfrom.position.축 > 변수)
    {
      Destroy(gameObject);
    } else if (transfrom.position.축 < 변수)
      {
        Destroy(gameObject);
      }
  }
</code>
</pre>


배열로 무작위 위치에 오브젝트 생성
-
<pre>
<code>
public GameObject [] 이름프리펩;
private float 스폰축 = 위치;
public float startDelay = 딜레이 시간;
public float spawnInterval = 스폰 시간;

void start()
  {
    InvokeRepeating("이름프리펩", startDelay, spawnInterval);
  }
void Update()
  {
    if (Input.GetKeyDown(KeyCode.누를 키))
    {
      스폰랜덤이름();
    }
void 스폰랜덤이름();
    {
      int 이름인덱스 = Random.Range(0, 이름인덱스.Length);
      Vector3 스폰이름 = new Vector3(Random.Range(-스폰축, 스폰축), y, 스폰이름);
      Instantiate(이름프리펩[이름인덱스], 스폰이름, 이름프리펩[이름인덱스].transform.rotation);
    }
  }
</code>
</pre>

특정위치에 오브젝트 생성
-
<pre>
<code>
public GameObject 스폰이름
private Vector3 spawnPos = new Vector3(x, y, z);
public float startDelay = 딜레이 시간;
public float spawnInterval = 스폰 시간;

void start()
  {
    InvokeRepeating("오브젝트스폰이름", startDelay, spawnInterval);
  }

void 오브젝트스폰이름()
{
  Instantiate(스폰이름, spawnPos, 스폰이름.transform.rotation);
}
</code>
</pre>

충돌 시 오브젝트 파괴
-
<pre>
<code>
void OnTriggerEnter(Collider.other)
  {
    //충돌시 콜라이더가 지정된 오브젝트와 그 오브젝트와 부딛힌 오브젝트 둘다 파괴
    Destroy.(gameObject);
    Destroy.(other.gameObject);
  }
</code>
</pre>

키를 눌러 점프 / 2단점프 방지 
-
<pre>
<code>
private Rigidbody playerRb;
//점프력과 중력 값 선언
public float jumpForce;
public float gravityModifier;
//땅에 뭍어있을 때만 점프 가능하도록 변수 선언
private bool inOnGround = true

void start()
{
  playerRb = GetComponent<Rigidbody>();
  Physics.gravity *= gravityModifier;
}

void update()
{
  if(Input.GetKeyDown(KeyCode.Space) && inOnGround)
    {
      playerRbAddForce(Vector3.up * jumpForce, ForceMode.Impulse);
      isOnGround = false;
    }
}

private void OnCollisionEnter(Conllision collision)
{
  isOnGround = true;
}
</code>
</pre>


  
