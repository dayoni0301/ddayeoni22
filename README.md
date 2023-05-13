<오픈소스 기초 프로젝트 - 일간 영화 순위 정보 제공>

정보통신공학부 2022039056 최다연
 
-프로젝트 소개 

날짜를 선택하면 해당 날짜의 박스오피스 순위와 누적 관람객 수를 화면에 보여주는 프로그램입니다. 또한, 화면에 있는 영화를 클릭하면 개봉일, 감독, 주연 정보를 함께 제공합니다.

영화 산업 관련 분야에서 유용한 프로젝트입니다. 일일 영화 흥행 순위와 감독, 주연 등의 정보를 제공함으로써 관객들이 영화를 선택하는 데에 도움을 줄 수 있습니다.

-개발 환경 및 언어

Visualstudiocode, javascript

-개발하고자하는 기능
확인하고자하는 날짜 지정
일일 박스오피스 순위 정보 제공
영화의 관련 세부정보 제공

- 소스코드

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script src="http://code.jquery.com/jquery-3.6.0.min.js"></script>
<script>
            $(function() {
                let y = new Date();
                y.setDate(y.getDate()-1);
                let str = y.getFullYear() + "-"
                + ("0" + (y.getMonth() + 1)).slice(-2) + "-"
                + ("0" + y.getDate()).slice(-2);
                $("#date").attr("max",str);

                // 버튼의 클릭 이벤트
                $("#mybtn").click(function() {
                    let d = $("#date").val();//YYYY-MM-dd
                    const regex = /-/g;
                    let d_str = d.replace(regex,"")//YYYYMMdd 

                    let url = "http://kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json?key=f5eef3421c602c6cb7ea224104795888&targetDt="+d_str

                     $.getJSON(url, function(data) {
                         let movieList = data.boxOfficeResult.dailyBoxOfficeList;
                         $("#boxoffice").empty();
                         $("#boxoffice").append(d+" 박스 오피스 순위<br>");
                         for(let i in movieList){
                             $("#boxoffice").append("<div class='movie' id="+movieList[i].movieCd+">"+(parseInt(i)+1)+". "+movieList[i].movieNm+" / "+movieList[i].audiAcc+"명</div><hr>");
                             console.log(movieList[i].movieCd);
                         }
                        });
                });//button click
                //영화 제목 클릭시 영화 정보 출력
                $("#boxoffice").on("click",".movie", function(){
                    let d = $(this);
                    let movieCd = d.attr("id");
                    let url = "http://www.kobis.or.kr/kobisopenapi/webservice/rest/movie/searchMovieInfo.json?key=f5eef3421c602c6cb7ea224104795888&movieCd="+movieCd;
                    $.getJSON(url,function(res){
                        let movie = res.movieInfoResult.movieInfo;
                        d.append("<hr>");
                        d.append("개봉일 : "+movie.openDt+"<br>");
                        d.append("감독 : "+movie.directors[0].peopleNm+"<br>");
                        d.append("주연 : "+movie.actors[0].peopleNm+", "+movie.actors[1].peopleNm+", "+movie.actors[2].peopleNm);
                        d.append("<hr>");

                    })
                })
            });//ready
        </script>

</head>
<body>
<input type="date" id="date"><button id="mybtn">확인</button>
<div id="boxoffice">
    박스 오피스 순위<br>
</div>
</body>
</html>
