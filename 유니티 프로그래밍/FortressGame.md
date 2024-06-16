![[FortressGame.png]]
![[FortressGame_2.png]]

# Inspectors
#### Tank
![[FortressGame_Tank.png]]

#### TargetPos
![[FortressGame_TargetPos.png]]

#### Ground
![[FortressGame_Ground.png]]

#### Bullet
![[FortressGame_Bullet.png]]

#### Effect
![[Fortress_Effect.png]]

#### MainCamera
![[Fortress_MainCamera.png]]

# Scripts
#### TankController
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class TankController : MonoBehaviour
{
    public Transform myTF;
    public Rigidbody2D rd;
    public float speed;

    public Transform bullet;
    public Transform targetPos;

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            Transform bullet = Instantiate(this.bullet);
            Rigidbody2D bulletRd = bullet.GetComponent<Rigidbody2D>();
            bullet.position = myTF.position + new Vector3(0, 1.5f, 0);
            bulletRd.velocity = GetInitialVelocity(bullet.position, targetPos.position);
        }

        Vector2 velocity = new Vector2(Input.GetAxisRaw("Horizontal") * speed, 0);
        rd.velocity = velocity;
    }

    Vector2 GetInitialVelocity(Vector2 start, Vector2 end, float maxHeightOffset = 0.6f, float rangeOffset = 0.11f)
    {
        Vector2 velocity = new Vector2();
        Vector2 direction = new Vector2(end.x - start.x, 0);

        float range = direction.magnitude;
        range += rangeOffset;
        Vector2 unitDirection = direction.normalized;

        float height = range / 2 + start.y;
        if (height < end.y) //구한 포물선의 높이가 도착지점 보다 낮다면 도착 지점의 높이에 고정 높이 가중치를 더한다.
        {
            height = end.y + maxHeightOffset;
        }

        velocity.y = Mathf.Sqrt(-2 * Physics2D.gravity.y * (height - start.y)); //초기 속도 Y
        //운동 에너지 = 위치 에너지이다. 이때, 운동 에너지가 가장 클 때는 처음 발사할 때이고, 위치 에너지가 가장 클 때는 물체가 가장 높은 곳에 있을 때이다.
        //그러므로, 물체가 가장 높이 떠있을 때의 힘을 구한다면 처음 발사할 때의 힘을 얻을 수 있다.
        //따라서, 처음 위치의 y좌표 + 포물선의 높이의 값을 가진 height에서 처음 위치의 y 좌표를 빼주어 포물선의 높이를 뽑고, 수식을 통해 최초 상태의 힘을 얻어낸다.

        float timeAscend = Mathf.Sqrt(-2 * (height - start.y) / Physics2D.gravity.y); //상승 시간
        float timeDescend = Mathf.Sqrt(-2 * (height - end.y) / Physics2D.gravity.y); //하강 시간
        float timeTotal = timeAscend + timeDescend; //총 비행 시간

        float velocityTotal = range / timeTotal; //평균 속도
        velocity.x = velocityTotal * unitDirection.x; //초기 속도 X

        return velocity;
    }
}
```

#### BulletController
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BulletController : MonoBehaviour
{
    public Transform myTF;
    public GameObject particle;
    public ParticleSystem effect;

    private void OnTriggerEnter2D(Collider2D collision)
    {
        if (collision.gameObject.tag.Equals("Bullet") == false)
        {
            GameObject myParticle = Instantiate(particle);
            effect = myParticle.GetComponent<ParticleSystem>();
            effect.transform.position = myTF.position;
            effect.transform.localScale = myTF.localScale;
            effect.Play();

            Destroy(gameObject);
        }
    }
}
```

#### TargetPosController
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class TargetPosController : MonoBehaviour
{
    public Transform tank;

    void Update()
    {
        Vector3 position = Camera.main.ScreenToWorldPoint(Input.mousePosition);
        position.y = tank.position.y;
        position.z = -1;
        transform.position = position;
    }
}
```

#### ParticleController
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using static UnityEngine.ParticleSystem;

public class ParticleController : MonoBehaviour
{
    void Start()
    {
        StartCoroutine( Remove() );
    }

    IEnumerator Remove()
    {
        yield return new WaitForSeconds(2f);
        Destroy(gameObject);
    }
}
```