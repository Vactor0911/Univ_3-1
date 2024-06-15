![[BallTrajectory.png]]

# Inspectors
#### GameManager
![[BallTrajectory_GameManager.png]]

#### Ball
![[BallTrajectory_Ball.png]]

#### Start
![[BallTrajectory_Start.png]]

#### End
![[BallTrajectory_End.png]]

# Scripts
#### GameManager
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GameManager : MonoBehaviour
{
    public Rigidbody ball;
    public Transform startTF;
    public Transform endTF;

    private void Update()
    {
        if ( Input.GetKeyDown(KeyCode.Space) )
        {
            ball.position = startTF.position + new Vector3(0, 0.5f, 0);
            ball.velocity = GetInitialVelocity(startTF.position, endTF.position);
        }
    }

    Vector3 GetInitialVelocity(Vector3 start, Vector3 end, float maxHeightOffset = 0.6f, float rangeOffset = 0.11f)
    {
        Vector3 velocity = new Vector3();
        Vector3 direction = new Vector3(end.x - start.x, 0, end.z - start.z); //방향
        float range = direction.magnitude; //거리
        range += rangeOffset;
        Vector3 unitDirection = direction.normalized; //노멀라이즈 된 방향 벡터

        float height = range / 2 + start.y; //실제 높이
        if (height < end.y) //구한 포물선의 높이가 도착지점 보다 낮다면 도착 지점의 높이에 고정 높이 가중치를 더한다.
        {
            height = end.y + maxHeightOffset;
        }

        velocity.y = Mathf.Sqrt( -2 * Physics.gravity.y * (height - start.y) ); //초기 속도 Y
        //운동 에너지 = 위치 에너지이다. 이때, 운동 에너지가 가장 클 때는 처음 발사할 때이고, 위치 에너지가 가장 클 때는 물체가 가장 높은 곳에 있을 때이다.
        //그러므로, 물체가 가장 높이 떠있을 때의 힘을 구한다면 처음 발사할 때의 힘을 얻을 수 있다.
        //따라서, 처음 위치의 y좌표 + 포물선의 높이의 값을 가진 height에서 처음 위치의 y 좌표를 빼주어 포물선의 높이를 뽑고, 수식을 통해 최초 상태의 힘을 얻어낸다.

        float timeAscend = Mathf.Sqrt(-2 * (height - start.y) / Physics.gravity.y); //상승 시간
        float timeDescend = Mathf.Sqrt(-2 * (height - end.y) / Physics.gravity.y); //하강 시간
        float timeTotal = timeAscend + timeDescend; //총 비행 시간

        float velocityTotal = range / timeTotal; //평균 속도
        velocity.x = velocityTotal * unitDirection.x; //초기 속도 X
        velocity.z = velocityTotal * unitDirection.z; //초기 속도 Z

        return velocity;
    }
}

```