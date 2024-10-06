# Unity Code

private와 public
-
<pre>
  변수를 선언할 때 private는 그 cs안에서만 사용되는 변수, public은 다른 cs에서 불러도 사용 되도록 만드는 변수이다

  오버라이드 변수를 선언할 때는 private나 public을 써주어야 하지만 커스텀 메소드에서는 구지 안 써줘도 된다
  커스텀 메소드에 private나 public을 쓰지 않으면 자동적으로 cs가 private로 만든다
</pre>

for()과 if()의 차이
-
<pre>
  for()은 몇번 반복할 것인지
  if()는 참인지 거짓인지를 보고 실행이 됨
</pre>

플레이어 이동
------
<pre>
* InputSystem을 이용하는 방법
<code>
using UnityEngine.InputSystem;
  
void Update()
(
  
    float horizontal = 0.0f;
    if (Keyboard.current.leftArrowKey.isPressed)
    {
        horizontal = -1.0f;
 	    }
    else if (Keyboard.current.rightArrowKey.isPressed)
    {
        horizontal = 1.0f;
    }
    Debug.Log(horizontal);

    Vector2 position = transform.position;
    position.x = position.x + 0.1f * horizontal;
    transform.position = position;
)
</code>
  
* 코드를 이용해 불러오는 방법  
<code>
[SerializeField] private float horsePower = 0;
private float speed;
private float turnspeed;
private float horizontalInput;
private float forwardInput;

void Update()
{
  //Horizontal은 키보드 좌,우(a,d) Vertical은 키보드 앞 뒤(w,s)를 불러옴
  horizontalInput = Input.GetAxis("Horizontal");
  forwardInput = Input.GetAxis("Vertical");

  /1. 
  //가해지는 힘만큼 이동속도가 올라감, Local을 사용해야 플레이어가 바라보는 방향으로 힘이 가해지는데 Local을 사용할려면 AddRelativeForce를 사용해야함, Global을 누루면 Local로 변함
  playerRb.AddRelativeForce(Vector3.forward * horsePower * verticalInput);
  //만약 플레이어나 탈것이 이동중 기울어지는 형상이 있다면 물리엔진에 중력원점을 낮추거나 변경해야한다
  
  

  
  /2.
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
public float xRange = ;
public float yRange = ;
public float zRange = ;

void Update()
  {
    if (transform.position.축 < ?Range)
      {
       //위치를 고정하고 싶으면 transform.position.축 대신 고정하고 싶은 위치를 적으면 그 위치 밖으로 못감
         transform.position.?칸에 ?Range가 들어가면 됨
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
    //특정 오브젝트와 충솔시 삭제를 하고 싶으면 if문으로 테그를 찾아야 함
    if(ohter.CompareTag("태그 이름"))
    {
      태그 이름 = true;
  
      //ohter.가 없으면 플레이어가, 있으면 다른 오브젝트가 삭제됨
      Destroy.(gameObject);
      Destroy.(other.gameObject);      
    }
    

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
      playerRb.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);
      isOnGround = false;
    }
}

private void OnCollisionEnter(Conllision collision)
{
  isOnGround = true;
}
</code>
</pre>

인스펙터 참조하기
-
<pre>
<code>
  GetComponent<참조할 인스펙터 이름>();
</code>
</pre>

문자출력
-
<pre>
<code>
  Debug.Log("문자");
</code>
</pre>

파티클 플레이
-
<pre>
<code>
  pubic ParticleSystem 파티클이름변수;
  
  //play, playOneShot 등이 가능
  파티클이름변수.play();
</code>
</pre>

이펙트 오디오 출력
-
<pre>
<code>
  private AudioSource 사운드이름변수; 
  public AudioClip 오디오클립이름변수;

  //뒤에 숫자는 1.0f = 100%를 뜻함 2.0f는 200%를 뜻함 
  사운드이름변수.PlayOneShot(오디오클립이름변수;, 1.0f);
</code>
</pre>

리지드바디를 이용한 플레이어 움직임
-
<pre>
private Rigidbody playerRb;
public float speed;

void Start()
{
  playerRb = GetComponent<Rigidbody>();
}

void Update()
{
  float forwardInput = Input.GetAxis("Vertical");
  float horizontalInput = Input.GetAxis("Horizontal");

  playerRb.AddForce(Vector3.forward * forwardInput * speed);
  playerRb.AddForce(Vector3.left * horizontalInput * speed);
}
</pre>

포컬포인트가 바라보는 방향에 따라 움직이기
-
<pre>
private GameObject focalPoint;
private Rigidbody playerRb;

void Start()
{
  playerRb = GetComponent<Rigidbody>();
  focalPoint = GameObject.Find("Focal Point");
}

void Update()
{
  float forwardInput = Input.GetAxis("Vertical");
  float horizontalInput = Input.GetAxis("Horizontal");

  playerRb.AddForce(focalPoint.transform.forward * forwardInput * speed);
  playerRb.AddForce(focalPoint.transform.left * horizontalInput * speed);
}
</pre>

적이 플레이어를 쫓아 움직임
-
<pre>
    public float speed = 3.0f;
    private Rigidbody enemyRb;
    private GameObject player;

    void Start()
    {
        enemyRb = GetComponent<Rigidbody>();
        player = GameObject.Find("Player");
    }

    void Update()
    {
        //normalized가 없으면 거리가 멀수록 가속도가 붙음, 있으면 정량화해 안붙음
        Vector3 lookDirection = (player.transform.position - transform.position).normalized; 

        enemyRb.AddForce(lookDirection * speed);
    }
</pre>

적이 플레이어의 반경 n이터의 위치에서 무작위로 생성
-
<pre>
    public GameObject enemyPrefabs;
    //여기서는 n을 9로함, 즉 플레이어 반경 9미터의 위치를 뜻함
    private float spawnRange = 9.0f;

    // Start is called before the first frame update
    void Start()
    {
        

        Instantiate(enemyPrefabs, GenerateSpawnPosition() , enemyPrefabs.transform.rotation);
    }

    // Update is called once per frame
    void Update()
    {
        
    }

    private Vector3 GenerateSpawnPosition()
    {
        float spwanPosX = Random.Range(-spawnRange, spawnRange);
        float spwanPosZ = Random.Range(-spawnRange, spawnRange);

        Vector3 randomPos = new Vector3(spwanPosX, 0, spwanPosZ);

        return randomPos;
    }
</pre>

충돌시 넉백 효과
-
<pre>
  private float powerUpStrenght = 10.0f;
  
  private void OnCollisionEnter(Collision collision)
    {
        if(collision.gameObject.CompareTag("Enemy") && hasPowerup)
        {
            //Enemy의 리지드바디를 가져옴
            Rigidbody enemyRigidbody = collision.gameObject.GetComponent<Rigidbody>();
            //충돌하면 어디로 가는지 방향을 정함
            Vector3 awayFromPlayer = collision.gameObject.transform.position - transform.position;

            //ForceMode.Impulse는 효과가 즉시 적용된다는 명령어이다
            enemyRigidbody.AddForce(awayFromPlayer * powerUpStrenght, ForceMode.Impulse);
            Debug.Log("Collided With: " + collision.gameObject.name + " with powerup set to " + hasPowerup);
        }
    }
</pre>

일정시간뒤 사라지는 카운트다운
-
<pre>
  private void OnTriggerEnter(Collider other)
    {
        if(other.CompareTag("Powerup"))
        {
            hasPowerup = true;
            Destroy(other.gameObject);
            //태그("Powerup")를 얻으면 카운트다운(PowerupCountdownRoutine)을 시작한다
            StartCoroutine(PowerupCountdownRoutine());
        }
    }

    IEnumerator PowerupCountdownRoutine()
    {
        //7초타이머를 시작한 후 끝나면 밑에있는 동작을 실행한다
        yield return new WaitForSeconds(7);
        hasPowerup = false;
    }
</pre>

버프 등의 시간제한 오브제를 표시
-
<pre>
  public GameObject powerupIndicator;

  void Update()
    {
        //오브제가 플레이어의 위치에서 y값 -0.5에 위치에서 따라다님
        powerupIndicator.transform.position = transform.position + new Vector3(0, -0.5f, 0);
    }
  
  private void OnTriggerEnter(Collider other)
    {
        if(other.CompareTag("Powerup"))
        {
            //트리거("Powerup")이 활성화되면 계층창에 표시를 꺼두었던 오브제를 활성화함
            powerupIndicator.gameObject.SetActive(true);
        }
    }

    IEnumerator PowerupCountdownRoutine()
    {
        yield return new WaitForSeconds(7);
        hasPowerup = false;
        //7초가 끝나면 다시 오브제를 비활성화해 모습을 감춤
        powerupIndicator.gameObject.SetActive(false);
    }
</pre>

for문을 이용해 WAVE에 따라 무작위 위치에 적을 생성
-
<pre>
  public GameObject enemyPrefabs;
  public int waveNumber = 1;
  
  void Update()
    {
        enemyCount = FindObjectsOfType<Enemy>().Length;

        if(enemyCount == 0)
        {
            waveNumber++;
            SpawnEnemyWave(waveNumber);
        }
    }

    void SpawnEnemyWave(int enemiesToSpawn)
    {
         for(int i = 0; i < enemiesToSpawn;  i++)
        {
            Instantiate(enemyPrefabs, GenerateSpawnPosition() , enemyPrefabs.transform.rotation);
        }
    }

    private Vector3 GenerateSpawnPosition()
    {
        float spwanPosX = Random.Range(-spawnRange, spawnRange);
        float spwanPosZ = Random.Range(-spawnRange, spawnRange);

        Vector3 randomPos = new Vector3(spwanPosX, 0, spwanPosZ);

        return randomPos;
    }
</pre>

물체가 움직일 때 Rigidbody를 이용해 회전
-
<pre>
  private Rigidbody targetRb;

  void Start()
    {
        targetRb = GetComponent<Rigidbody>();
        //Force는 물체가 받는 힘, Vector뒤에 up은 물체가 받는 힘의 방향, 받는 힘은 12에서 16사이의 힘을 받고 받는 힘을 바로 적용해야 하니 ForceMode중 Impulse를 사용 
        targetRb.AddForce(Vector3.up * Random.Range(12, 16), ForceMode.Impulse);
        //Torque는 물체를 회전시키는 메소드고 각각 x,y,z,방향으로 회전하는 힘이 적용된다 동일하게 힘이 바로 적용되도록 Impulse를 사용
        targetRb.AddTorque(Random.Range(-10, 10), Random.Range(-10, 10) , Random.Range(-10, 10), ForceMode.Impulse );
    }
</pre>

List<> 선언
-
<pre>
  public / private List<GameObject> 변수이름;

public class GameManager : MonoBehaviour
{
    // '타겟'오브젝트를 리스트로 모아둠
    public List<GameObject> targets;
    //스폰시간은 1프레임(1초)
    public float SpawnRate = 1.0f;
    void Start()
    {
        //코루틴을 시작한다
        StartCoroutine(SpawnTarget());   
    }

    IEnumerator SpawnTarget()
    {
        while(true)
        {
            yield return new WaitForSeconds(SpawnRate);
            //인덱스에 0부터 '타겟'오브젝트에 설정한 수까지를 랜덤으로 넣음
            int index = Random.Range(0, targets.Count);
            Instantiate(targets[index]);
        }
    } 
</pre>

마우스 클릭 선언
-
<pre>
  //마우스 클릭은 스페이스나 다른 키와 다르게 정해진 오버라이드 변수이다
    GetKey처럼 Donw, up을 쓸 수 있다
  private void OnMouseDown()
  {

  }
</pre>

텍스트 메스 프로 사용해 클릭해 파괴한 오브젝트 점수 표현
-
<pre>
  [Game Manager.cs]
  
  using TMPro;

  public TextMeshProUGUI scoreText;
  
  void Start()
    {
        StartCoroutine(SpawnTarget());
        score = 0;
        UpdateScore(0);
    }

  public void UpdateScore(int scoreToAdd)
    {
        score += scoreToAdd;
        scoreText.text = "Score:" + score;
    }

  [Target.cs]

  public GameManager gameManager;

  private void OnMouseDown()
    {
        Destroy(gameObject);
        gameManager.UpdateScore(5);
    }
</pre>

오브젝트를 클릭했을 때 오브젝트가 삭제되며 파티클효과 나타내기
-
<pre>
  public ParticleSystem explosionParticle;

  private void OnMouseDown()
    {
        Destroy(gameObject);
        Instantiate(explosionParticle, transform.position, explosionParticle.transform.rotation);
        gameManager.UpdateScore(pointValue);
    }
</pre>

게임 오버 메세지 출력과 씬 리셋
-
<pre>
  using UnityEngine.SceneManagement;
  using UnityEngine.UI;
  //게임메니저에 퍼블릭으로 변수를 만들었으면 꼭 게임메니저 오브젝트의 인스펙터 창에서 연결을 시켜줘야함
  public TextMeshProUGUI gameOvertext;
  public Button restartButton;

  void Start()
    {
        isGameActive = true;
        score = 0;

        StartCoroutine(SpawnTarget());
        UpdateScore(0);
    }
  
  public void GameOver()
    {
        restartButton.gameObject.SetActive(true);
        gameOvertext.gameObject.SetActive(true);
        isGameActive = false;
    }

    public void RestartGame()
    {
        //넘어갈 씬을 네임스페이스로 찾겠다는 메소드
        SceneManager.LoadScene(SceneManager.GetActiveScene().name);
    }
</pre>


Coroutine
---------------
특정 시간 간격으로 반복 실행하거나, 비동기 작업을 처리할 때 유용하게 사용됩니다

1. 일시 중지 및 재개: 코루틴은 특정 조건이 충족될 때까지 실행을 일시 중지할 수 있으며, 이후에 다시 실행을 재개할 수 있습니다. 이를 통해 긴 작업을 여러 프레임에 걸쳐 나눠서 실행할 수 있습니다.<br>
2. IEnumerator 반환형: 코루틴 메서드는 IEnumerator를 반환해야 합니다. 이 반환형은 유니티 엔진이 코루틴의 실행을 관리하는 데 사용됩니다.<br>
3.yield 문: yield return 문을 사용하여 코루틴의 실행을 일시 중지할 수 있습니다. yield return null은 다음 프레임까지 실행을 일시 중지하고, yield return new WaitForSeconds(1f)는 1초 후에 실행을 재개합니다.

<>와 []
------------
* <>는 리스트고 []는 배열이다

public / private / const / protected / readonly / static / [SerializeField]
--------
* const: constant의 약자로 public/private 뒤에 선언하면 그 변수의 값을 절대로 바꿀 수 없게 된다<br>
ex) private const float speed = 10; //const를 사용하면 어떤 지역에서도 speed의 값을 10에서 변경이 불가능하다<br>
* protected : private + public , 이 스크립트, 이 클래스에서 변수를 사용할 수 있지만, 이 클래스를 상속받는 다른 스크립트 에서도 사용할 수 있다. 현재클래스 외부의 변수나 메서드를 볼수 있다. 하지만 이는 이 메인 클레스에서 상속받는 스크립트나 클래스에만 해당한다.<br>
* readonly : 처음으로 오브젝트에 Instantiate() 를 적용 할 때, 해당 오브젝트를 만들때 변수를 설정할 수 있지만, 그 변수를 다시 변경 할 수는 없다.<br>
* static : 코드 어디든 전역변수를 넣을 수 있다.<br>
* [SerializeField]: 인스펙터창에 표시되지 않는 private 변수들을 인스펙터 창에 표시해 그 지역 내에서만 변경하며 사용하도록 설정해주는 메소드이다<br>
ex) 만약 같은 playerContoller라는 스크립트를 사용하는 A와 B에서 A의 속도만 바꾸고 싶다면 [SerializeField]를 설정하고 A의 인스펙터창에서 값을 변경하면 된다<br>

Fixed/Late Update
--------
* FixedUpdate: 움직임이나 물리를 계산할 때 사용됨
* LateUpdate: 플레이어를 따라가는 카메라에 많이 사용됨

Awake
----
* Awake: 게임이 시작될 때가 아니라 오브젝트가 동작하는 경우에 많이 사용함
ex) 누군가 버튼을 누루면 벽이 어떤 동작을 수행할 때

Exsit Package
-
게임을 페키지 형태로내볼때 file 에서 누루면 됨

오브젝트 풀링
----------
* 오브젝트 풀링: 오브젝트를 특정 개수 만큼 선택하여 온오프 할 수 있음

제작하는 프로그램의 모듈을 선택
-
1. 제작할 프로젝트를 연다
2. file > Build Settings > Add Open Scenes로 씬을 추가
3. Player Settings에서 원하는 설정을 조정
4. Build를 클릭
5. Platform에는 윈도우, 안드로이드. 플스 등등이 있다

Vector
-
* Vector뒤에 오는 숫자는 저장할 수 있는 숫자 값이다 만약 2D게임이라면 Vector2, 3D게임이라면 Vector3가 적절하다

유니티 퀴즈 오답노트!
----
* bool값은 기본적으로 false상태이다
* 정답은 100(99이하일때 실행되는 조건이기에 그렇다는데 솔직히 잘 모르겠음;; 아마 99'이하'일때 니까 99에서도 +1해서 100인듯함)
~~~C#
int count = 0;
void OnMouseDrag()
{
  if(count < 100)
  {
    count++
  }
}
~~~








