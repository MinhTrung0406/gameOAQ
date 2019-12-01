# gameOAQ
game Ô Ăn Quan

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Net;
using System.Net.Sockets;
using System.IO;
using System.Runtime.Serialization.Formatters.Binary;
using System.Threading;

namespace GameOAQ13
{
    public partial class Server : Form
    {
        private static int TIMER_DELAY = 1000;
        private int time_tick = 15; // Đếm thời gian
        private bool chay = false;
        private bool ben_trai = false;
        private int thoigian = 0;
        private int total_An = 0;
        bool bot = false;
        private int[] banchoi = new int[] { 0, 5, 5, 5, 5, 5, 0, 5, 5, 5, 5, 5 };
        private bool chua1 = true;
        private bool chua2 = true;
        private int totalquan_play1 = 0;
        private int totalquan_play2 = 0;
        private int totalchua_play1 = 0;
        private int totalchua_play2 = 0;
        private bool Player1 = true;
        private bool Player2 = false;
        public bool AutoChoi = true;
        public bool Easy = true;
        public bool Normal = false;
        private System.String name = "MÁY";
        private int OdangChon = -1;
        private int TotalChon = 0;


        public Server()
        {
            InitializeComponent();
            reload();
        }

        

        private void Server_Load(object sender, EventArgs e)
        {
            
        }

        private void player1_trai_Click(object sender, EventArgs e)
        {
            if (OdangChon >= 1 && OdangChon <= 5)
            {
                banchoi[OdangChon] = 0;
                //  bentrai(OdangChon-1);
                chay = true;
                ben_trai = true;
                setAllbtn();
            }

            if (chay)
            {
                NuocDi(OdangChon);
            }
            if (AutoChoi && thoigian >= 3 && bot)
            {
                autochon();
                thoigian = 0;
                bot = false;
            }
            thoigian++;
            if (time_tick < 0)
            {
                //  reload_Luot();
                Randomchon();
                time_tick = 15;
            }
            if (!chay)
            {
                txt_time.Text = time_tick.ToString();
                time_tick--;
            }
        }

        private void player1_phai_Click(object sender, EventArgs e)
        {
            if (OdangChon >= 1 && OdangChon <= 5)
            {
                banchoi[OdangChon] = 0;
                //  bentrai(OdangChon-1);
                chay = true;
                ben_trai = false;
                setAllbtn();
            }

            if (chay)
            {
                NuocDi(OdangChon);
            }
            if (AutoChoi && thoigian >= 3 && bot)
            {
                autochon();
                thoigian = 0;
                bot = false;
            }
            thoigian++;
            if (time_tick < 0)
            {
                //  reload_Luot();
                Randomchon();
                time_tick = 15;
            }
            if (!chay)
            {
                txt_time.Text = time_tick.ToString();
                time_tick--;
            }
        }

        private void player2_trai_Click(object sender, EventArgs e)
        {
            if (OdangChon >= 7 && OdangChon <= 11)
            {
                banchoi[OdangChon] = 0;
                //  bentrai(OdangChon-1);
                chay = true;
                ben_trai = true;
                setAllbtn();
            }
        }

        private void player2_phai_Click(object sender, EventArgs e)
        {
            if (OdangChon >= 7 && OdangChon <= 11)
            {
                banchoi[OdangChon] = 0;
                //  bentrai(OdangChon-1);
                chay = true;
                ben_trai = false;




                setAllbtn();

            }
        }

        private void bt_ChoiLai_Click(object sender, EventArgs e)
        {
            DialogResult dlr = new DialogResult();
            dlr = MessageBox.Show("Bạn có muốn chơi lại không ?", "Thông Báo", MessageBoxButtons.YesNo);

            if (dlr == DialogResult.Yes)
            {
                if (name.Length < 5)
                {
                    AutoChoi = true;
                }
                chay = false;
                reload();
            }
        }

        private void bt_Thoat_Click(object sender, EventArgs e)
        {
            DialogResult dlr = new DialogResult();
            dlr = MessageBox.Show("Bạn có muốn thoát không ?", "Thông Báo", MessageBoxButtons.YesNo);

            if (dlr == DialogResult.Yes)
            {
                Application.Exit();
            }

        }


        private void picBox_Quan1_Click(object sender, EventArgs e)
        {
            // Luu y
            if (!chay)
            {
                if (Player1)
                {
                    if (banchoi[1] > 0)
                    {
                        OdangChon = 1;
                        TotalChon = banchoi[1];
                        updatethongbao();
                        UpdateImg(1);
                        setbtnPlayer1();
                    }
                }
                else
                {
                    MessageBox.Show("ô không hợp lệ", "Thông Báo");
                }
            }
        }

        private void picBox_Quan2_Click(object sender, EventArgs e)
        {
            if (!chay)
                if (Player1)
                {
                    if (banchoi[2] > 0)
                    {
                        OdangChon = 2;
                        TotalChon = banchoi[2];
                        updatethongbao();
                        UpdateImg(2);
                        setbtnPlayer1();
                    }
                }
                else
                {
                    MessageBox.Show("ô không hợp lệ", "Thông Báo");
                }
        }

        private void picBox_Quan3_Click(object sender, EventArgs e)
        {
            if (!chay)
            {
                if (Player1)
                {
                    if (banchoi[3] > 0)
                    {
                        OdangChon = 3;
                        TotalChon = banchoi[3];
                        updatethongbao();
                        UpdateImg(3);
                        setbtnPlayer1();
                    }
                }
                else
                {
                    MessageBox.Show("ô không hợp lệ", "Thông Báo");
                }
            }
        }

        private void picBox_Quan4_Click(object sender, EventArgs e)
        {
            if (!chay)
            {
                if (Player1)
                {
                    if (banchoi[4] > 0)
                    {
                        OdangChon = 4;
                        TotalChon = banchoi[4];
                        updatethongbao();
                        UpdateImg(4);
                        setbtnPlayer1();
                    }
                }
                else
                {
                    MessageBox.Show("ô không hợp lệ", "Thông Báo");
                }
            }
        }

        private void picBox_Quan5_Click(object sender, EventArgs e)
        {
            if (!chay)
            {
                if (Player1)
                {
                    if (banchoi[5] > 0)
                    {
                        OdangChon = 5;
                        TotalChon = banchoi[5];
                        updatethongbao();
                        UpdateImg(5);
                        setbtnPlayer1();
                    }
                }
                else
                {
                    MessageBox.Show("ô không hợp lệ", "Thông Báo");
                }
            }
        }

        private void picBox_Quan6_Click(object sender, EventArgs e)
        {
            if (!chay)
            {
                if (Player2 && !AutoChoi)
                {
                    if (banchoi[11] > 0)
                    {
                        OdangChon = 11;
                        TotalChon = banchoi[11];
                        updatethongbao();
                        UpdateImg(11);
                        setbtnPlayer2();
                    }
                }
                else
                {
                    MessageBox.Show("ô không hợp lệ", "Thông Báo");
                }
            }
        }

        private void picBox_Quan7_Click(object sender, EventArgs e)
        {
            if (!chay)
            {
                if (Player2 && !AutoChoi)
                {
                    if (banchoi[11] > 0)
                    {
                        OdangChon = 11;
                        TotalChon = banchoi[11];
                        updatethongbao();
                        UpdateImg(11);
                        setbtnPlayer2();
                    }
                }
                else
                {
                    MessageBox.Show("ô không hợp lệ", "Thông Báo");
                }
            }
        }

        private void picBox_Quan8_Click(object sender, EventArgs e)
        {
            if (!chay)
            {
                if (Player2 && !AutoChoi)
                {
                    if (banchoi[11] > 0)
                    {
                        OdangChon = 11;
                        TotalChon = banchoi[11];
                        updatethongbao();
                        UpdateImg(11);
                        setbtnPlayer2();
                    }
                }
                else
                {
                    MessageBox.Show("ô không hợp lệ", "Thông Báo");
                }
            }
        }

        private void picBox_Quan9_Click(object sender, EventArgs e)
        {
            if (!chay)
            {
                if (Player2 && !AutoChoi)
                {
                    if (banchoi[11] > 0)
                    {
                        OdangChon = 11;
                        TotalChon = banchoi[11];
                        updatethongbao();
                        UpdateImg(11);
                        setbtnPlayer2();
                    }
                }
                else
                {
                    MessageBox.Show("ô không hợp lệ", "Thông Báo");
                }
            }
        }

        private void picBox_Quan10_Click(object sender, EventArgs e)
        {
            if (!chay)
            {
                if (Player2 && !AutoChoi)
                {
                    if (banchoi[11] > 0)
                    {
                        OdangChon = 11;
                        TotalChon = banchoi[11];
                        updatethongbao();
                        UpdateImg(11);
                        setbtnPlayer2();
                    }
                }
                else
                {
                    MessageBox.Show("ô không hợp lệ", "Thông Báo");
                }
            }
        }


        public void reload()
        {
            banchoi = new int[] { 0, 5, 5, 5, 5, 5, 0, 5, 5, 5, 5, 5 };
            chua1 = true;
            chua2 = true;
            totalquan_play1 = 0;
            totalquan_play2 = 0;
            totalchua_play1 = 0;
            totalchua_play2 = 0;
            Player1 = true;
            Player2 = false;
            time_tick = 15;

            if (AutoChoi)
            {
                name = "Máy";
                lb_name.Text = "Máy";
                AutoChoi = !AutoChoi;
                if (Easy)
                {
                    lb_ttmucdo.Text = "Dễ";
                }
                else if (Normal)
                {
                    lb_ttmucdo.Text = "Thường";
                }
                else
                {
                    lb_ttmucdo.Text = "khó";
                }

            }
            else
            {
                name = "Player2";
                lb_name.Text = "Người - Người";
                lb_ttmucdo.Text = "Người chơi 2";
            }

            for (int i = 0; i < 12; i++)
            {
                UpdateImg(i); // updateImage
            }

            OdangChon = -1;
            updatevaluse();
            updatethongbao();
            setAllbtn();
        }

        private void Playergame()
        {
            Player1 = !Player1;
            Player2 = !Player2;

            if (name.Length < 5)
            {
                AutoChoi = !AutoChoi;
            }
        }

        // Kiem tra chien thang + value :Ham nay chua lam ***************************************************************
        private void checkwin()
        {
            if (!chua1 && !chua2 && banchoi[0] == 0 && banchoi[6] == 0)
            {
                int a, b, aa, bb;

                aa = banchoi[1] + banchoi[2] + banchoi[3] + banchoi[4] + banchoi[5];
                bb = banchoi[7] + banchoi[8] + banchoi[9] + banchoi[10] + banchoi[11];

                DialogResult dlr = new DialogResult();
                if ((a = (totalquan_play1 + totalchua_play1 * 10 + aa)) > (b = (totalquan_play2 + totalchua_play2 * 10 + bb)))
                {
                    dlr = MessageBox.Show("PLAYER 1 WIN ( " + a.ToString() + " : " + b.ToString() + " )", "Thông báo", MessageBoxButtons.OK);
                }
                else
                {
                    dlr = MessageBox.Show(name + " WIN ( " + b.ToString() + " : " + a.ToString() + " )", "Thông báo", MessageBoxButtons.OK);
                }

                dlr = MessageBox.Show("Bạn có muốn chơi tiếp ?", "Thông báo", MessageBoxButtons.YesNo);
                if (dlr == DialogResult.No)
                {
                    Application.Exit();
                }
                else
                {
                    if (name.Length < 5)
                    {

                        AutoChoi = true;
                    }
                    chay = false;
                    reload();
                }
            }
        }

        //Lay gia tri. Suy nghi ki ham` nay`
        private bool getvaluse(int min, int max)
        {
            for (int i = min; i <= max; i++)
            {
                if (banchoi[i] > 0)
                {
                    return true;
                }
            }
            return false;
        }

        // Kiem tra chien thăng
        private void checkvalueplayer()
        {
            if (Player1)
            {
                if (totalquan_play1 < 5 && totalchua_play1 == 0)
                {
                    if (!getvaluse(1, 5))
                    {
                        DialogResult dlr = MessageBox.Show(name + " win", "ThongBao", MessageBoxButtons.OK);
                        if (dlr == DialogResult.OK)
                        {

                            DialogResult dlr2 = MessageBox.Show("Bạn có muốn chơi tiếp ?", "Thông báo", MessageBoxButtons.YesNo);

                            if (dlr2 == DialogResult.Yes)
                            {

                                if (name.Length < 5)
                                {
                                    AutoChoi = true;
                                }
                                chay = false;
                                reload();
                            }
                            if (dlr2 == DialogResult.No)
                            {
                                Application.Exit();
                            }

                        }
                        else
                        {
                            Application.Exit();
                        }
                    }
                }

                else if (totalquan_play1 > 4)
                {
                    if (!getvaluse(1, 5))
                    {
                        int dem = 1;
                        while (totalquan_play1 > 0 && dem < 6)
                        {
                            banchoi[dem]++;
                            UpdateImg(dem);
                            dem++;
                            totalquan_play1--;
                        }
                        updatevaluse();
                    }
                }

                else if (totalchua_play1 > 0)
                {
                    if (!getvaluse(1, 5))
                    {
                        totalchua_play1--;
                        totalquan_play1 += 10;
                        int dem = 1;

                        while (totalquan_play1 > 0 && dem < 6)
                        {
                            banchoi[dem]++;
                            UpdateImg(dem);
                            dem++;
                            totalquan_play1--;
                        }
                        updatevaluse();
                    }
                }
            }
            else
            {
                if (totalquan_play2 < 5 && totalchua_play2 == 0)
                {
                    if (!getvaluse(7, 11))
                    {
                        DialogResult dlr = MessageBox.Show(name + " win", "ThongBao", MessageBoxButtons.OK);
                        if (dlr == DialogResult.OK)
                        {

                            DialogResult dlr2 = MessageBox.Show("Bạn có muốn chơi tiếp ?", "Thông báo", MessageBoxButtons.YesNo);

                            if (dlr2 == DialogResult.Yes)
                            {

                                if (name.Length < 5)
                                {
                                    AutoChoi = true;
                                }
                                chay = false;
                                reload();
                            }
                            else
                            {
                                Application.Exit();
                            }

                        }
                        else
                        {
                            Application.Exit();
                        }
                    }
                }
                else if (totalquan_play2 > 4)
                {
                    if (!getvaluse(7, 11))
                    {
                        int dem = 7;

                        while (totalquan_play2 > 0 && dem < 12)
                        {
                            banchoi[dem]++;
                            UpdateImg(dem);
                            dem++;
                            totalquan_play2--;
                        }
                        updatevaluse();
                    }
                }
                else if (totalchua_play2 > 0)
                {
                    if (!getvaluse(7, 11))
                    {
                        totalchua_play2--;
                        totalquan_play2 += 10;
                        int dem = 7;

                        while (totalquan_play2 > 0 && dem < 12)
                        {
                            banchoi[dem]++;
                            UpdateImg(dem);
                            dem++;
                            totalquan_play2--;
                        }
                        updatevaluse();
                    }
                }
            }
        }



        //Luu y cho up anh
        private void UpdateImg(int a)
        {
            switch (a)
            {
                case 0:
                    {
                        Image icon;
                        if (banchoi[a] > 5 && chua1 == true)
                        {
                            
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nchua1.jpg";
                            
                            icon = Image.FromFile(file);
                            
                        }

                        else if (banchoi[a] > 5 && chua1 == false)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nchua0.jpg";
                            icon = Image.FromFile(file);
                        }
                        else if (chua1 == true)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "chua1.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "chua0.jpg";
                            icon = Image.FromFile(file);
                        }
                        // Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quanto1.Text = bla;
                        banchoi[a].ToString();

                        //Chuyển đổi ảnh
                        picBox_Quanto1.Image = icon;
                        break;
                    }
                case 1:
                    {
                        Image icon;
                        if (banchoi[a] > 7)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan1.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan1.Image = icon;
                        break;
                    }
                case 2:
                    {
                        Image icon;
                        if (banchoi[a] > 7)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan2.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan2.Image = icon;
                        break;
                    }
                case 3:
                    {
                        Image icon;
                        if (banchoi[a] > 7)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan3.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan3.Image = icon;
                        break;
                    }
                case 4:
                    {
                        Image icon;
                        if (banchoi[a] > 7)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan4.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan4.Image = icon;
                        break;
                    }
                case 5:
                    {
                        Image icon;
                        if (banchoi[a] > 7)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan5.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan5.Image = icon;
                        break;
                    }
                case 7:
                    {
                        Image icon;
                        if (banchoi[a] > 7)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan6.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan6.Image = icon;
                        break;
                    }
                case 8:
                    {
                        Image icon;
                        if (banchoi[a] > 7)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan7.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan7.Image = icon;
                        break;
                    }
                case 9:
                    {
                        Image icon;
                        if (banchoi[a] > 7)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan8.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan8.Image = icon;
                        break;
                    }
                case 10:
                    {
                        Image icon;
                        if (banchoi[a] > 7)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan9.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan9.Image = icon;
                        break;
                    }
                case 11:
                    {
                        Image icon;
                        if (banchoi[a] > 7)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan10.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan10.Image = icon;
                        break;
                    }
                case 6:
                    {
                        Image icon;
                        if ((banchoi[a] > 5) && (chua1 == true))
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nchua2.jpg";
                            icon = Image.FromFile(file);
                        }

                        else if (banchoi[a] > 5 && chua1 == false)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nchua0.jpg";
                            icon = Image.FromFile(file);
                        }
                        else if (chua2 == true)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "chua2.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "chua0.jpg";
                            icon = Image.FromFile(file);
                        }
                        // Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quanto2.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quanto2.Image = icon;
                        break;
                    }
                default:
                    break;
            }
        }

        private void updateImageBoc(int a)
        {
            switch (a)
            {
                case 0:
                    {
                        Image icon;
                        if ((banchoi[a] > 5) && (chua1 == true))
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nchua1.jpg";
                            icon = Image.FromFile(file);
                        }

                        else if (banchoi[a] > 5 && chua1 == false)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nchua0.jpg";
                            icon = Image.FromFile(file);
                        }
                        else if (chua1 == true)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "chua1.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "chua0.jpg";
                            icon = Image.FromFile(file);
                        }

                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quanto1.Text = bla;

                        picBox_Quanto1.Image = icon;
                        break;
                    }

                case 1:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan1.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan1.Image = icon;
                        break;
                    }
                case 2:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan2.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan2.Image = icon;
                        break;
                    }
                case 3:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan3.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan3.Image = icon;
                        break;
                    }
                case 4:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan4.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan4.Image = icon;
                        break;
                    }
                case 5:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan5.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan5.Image = icon;
                        break;
                    }
                case 7:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan6.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan6.Image = icon;
                        break;
                    }
                case 8:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan7.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan7.Image = icon;
                        break;
                    }
                case 9:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan8.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan8.Image = icon;
                        break;
                    }
                case 10:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan9.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan9.Image = icon;
                        break;
                    }
                case 11:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nquan.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "quan.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan10.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan10.Image = icon;
                        break;
                    }
                case 6:
                    {
                        Image icon;
                        if ((banchoi[a] > 5) && (chua1 == true))
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nchua2.jpg";
                            icon = Image.FromFile(file);
                        }

                        else if (banchoi[a] > 5 && chua1 == false)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\nchua0.jpg";
                            icon = Image.FromFile(file);
                        }
                        else if (chua2 == true)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "chua2.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\" + banchoi[a] + "chua0.jpg";
                            icon = Image.FromFile(file);
                        }
                        // Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quanto2.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quanto2.Image = icon;
                        break;
                    }
                default:
                    break;
            }
        }

        private void updateImageAn(int a)
        {
            switch (a)
            {

                case 0:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua1.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua2.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quanto1.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quanto1.Image = icon;
                        break;
                    }

                case 1:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua1.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua2.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan1.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan1.Image = icon;
                        break;
                    }
                case 2:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua1.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua2.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan2.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan2.Image = icon;
                        break;
                    }
                case 3:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua1.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua2.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan3.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan3.Image = icon;
                        break;
                    }
                case 4:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua1.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua2.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan4.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan4.Image = icon;
                        break;
                    }
                case 5:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua1.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua2.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan5.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan5.Image = icon;
                        break;
                    }
                case 7:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua1.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua2.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan6.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan6.Image = icon;
                        break;
                    }
                case 8:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua1.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua2.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan7.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan7.Image = icon;
                        break;
                    }
                case 9:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua1.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua2.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan8.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan8.Image = icon;
                        break;
                    }
                case 10:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua1.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua2.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan9.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan9.Image = icon;
                        break;
                    }
                case 11:
                    {
                        Image icon;
                        if (Player1)
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua1.jpg";
                            icon = Image.FromFile(file);
                        }
                        else
                        {
                            string file = @"C:\Users\quocm\source\repos\GameOAQ13\GameOAQ13\Image\anchua2.jpg";
                            icon = Image.FromFile(file);
                        }
                        //Cập nhật số quân
                        string bla;
                        bla = Convert.ToString(banchoi[a]);
                        lb_Quan10.Text = bla;

                        //Chuyển đổi ảnh
                        picBox_Quan10.Image = icon;
                        break;
                    }
            }
        }

        private void updatethongbao()
        {
            if (OdangChon != -1 || chay)
            {
                txtthongbao.Text = "Số Dân";
                txtSoQuan.Text = "       " + TotalChon.ToString();
            }
            else
            {
                txtthongbao.Text = "Lượt Của";
                if (Player1)
                {
                    txtSoQuan.Text = "PLAYER 1";
                }
                else
                {
                    bot = true;
                    txtSoQuan.Text = "    " + name;
                }
            }
        }

        private void updatevaluse()
        {
            txtchuaplayer1.Text = totalchua_play1.ToString();
            txtchuaplayer2.Text = totalchua_play2.ToString();
            txtquanplayer1.Text = totalquan_play1.ToString();
            txtquanplayer2.Text = totalquan_play2.ToString();
        }

        private int uploadvitri(int i)
        {
            if (ben_trai)
            {
                if (i == 0)
                {
                    i = 11;
                }
                else
                {
                    i--;
                }
            }
            else
            {
                if (i == 11)
                {
                    i = 0;
                }
                else
                {
                    i++;
                }
            }
            return i;
        }

        private void NuocDi(int vitri)
        {
            int i = uploadvitri(vitri);
            if (TotalChon > 0)
            {
                banchoi[i] += 1;
                TotalChon--;
                updatethongbao();

                if (ben_trai)
                {
                    UpdateImg((i == 11) ? 0 : (i + 1));
                }
                else
                {
                    UpdateImg((i == 0) ? 11 : (i - 1));
                }

                UpdateImg(i);
                i = uploadvitri(i);
                if (TotalChon == 0 && banchoi[i] > 0 && i != 0 && i != 6)
                {
                    TotalChon = banchoi[i];
                    banchoi[i] = 0;
                    updateImageBoc(i);
                    i = uploadvitri(i);
                }
                else if (TotalChon == 0 && banchoi[i] == 0)
                {
                    if (i != 0 && i != 6)
                    {
                        while (banchoi[i] == 0 && i != 0 && i != 6)
                        {
                            i = uploadvitri(i);
                            if (banchoi[i] > 0)
                            {
                                if (i == 0)
                                {
                                    if (Player1)
                                    {
                                        totalquan_play1 += banchoi[i];
                                        if (chua1)
                                        {
                                            totalchua_play1++;
                                        }
                                        chua1 = false;
                                    }
                                    else
                                    {
                                        totalquan_play2 += banchoi[i];
                                        if (chua1)
                                        {
                                            totalchua_play2++;
                                        }
                                        chua1 = false;
                                    }
                                }
                                else if (i == 6)
                                {
                                    if (Player1)
                                    {
                                        totalquan_play1 += banchoi[i];
                                        if (chua2)
                                        {
                                            totalchua_play1++;
                                        }
                                        chua2 = false;
                                    }
                                    else
                                    {
                                        totalquan_play2 += banchoi[i];
                                        if (chua2)
                                        {
                                            totalchua_play2++;
                                        }
                                        chua2 = false;
                                    }
                                }
                                else
                                {
                                    if (Player1)
                                    {
                                        totalquan_play1 += banchoi[i];
                                    }
                                    else
                                    {
                                        totalquan_play2 += banchoi[i];
                                    }
                                }
                                banchoi[i] = 0;
                                updateImageAn(i);
                                i = uploadvitri(i);
                            }
                            else
                            {
                                break;
                            }
                        }
                    }
                }
                updatevaluse();
                if (ben_trai)
                    OdangChon = i + 1;
                else
                    OdangChon = i - 1;
            }
            else
            {
                reload_Luot();
            }
        }

        private void reload_Luot()
        {
            for (int i = 0; i < 12; i++)
            {
                UpdateImg(i);
            }

            updatevaluse();
            TotalChon = 0;
            OdangChon = -1;
            chay = false;
            Playergame();
            updatethongbao();
            checkwin();
            checkvalueplayer();
            thoigian = 0;
            time_tick = 15;
        }

        /*public void BaoCao()
        {
            reload();
        }*/

        private void autochon()
        {
            if (Easy)
            {
                Randomchon();
            }
            else
            {
                int[] tam = new int[] { banchoi[0], banchoi[1], banchoi[2], banchoi[3], banchoi[4], banchoi[5], banchoi[6], banchoi[7], banchoi[8], banchoi[9], banchoi[10], banchoi[11] };
                if (Normal)
                {
                    AI TriTue = new AI();
                    TriTue.banchoi1 = tam;
                    TriTue.nquan1 = chua1;
                    TriTue.nquan2 = chua1;
                    TriTue.autochonAI();
                    if (TriTue.total_An > 0)
                    {
                        OdangChon = TriTue.dachon;

                        TotalChon = banchoi[OdangChon];
                        updatethongbao();
                        UpdateImg(OdangChon);
                        banchoi[OdangChon] = 0;
                        ben_trai = TriTue.trai;
                        chay = true;
                    }
                    else
                    {
                        Randomchon();
                    }
                }
            }
        }

        private void Randomchon()
        {
            Random rd = new Random();
            bool thanhcong = false;

            while (!thanhcong)
            {
                if (Player2)
                {
                    int ii = rd.Next(12);
                    if (ii > 6)
                    {
                        TotalChon = banchoi[ii];
                        if (TotalChon > 0)
                        {
                            OdangChon = ii;
                            updatethongbao();
                            UpdateImg(ii);
                            thanhcong = true;
                        }
                    }
                }
                else
                {
                    int ii = rd.Next(6);
                    if (ii > 0)
                    {
                        TotalChon = banchoi[ii];
                        if (TotalChon > 0)
                        {
                            OdangChon = ii;
                            updatethongbao();
                            UpdateImg(ii);
                            thanhcong = true;
                        }
                    }
                }
            }
            int i = rd.Next(2);
            if (i == 0)
            {
                banchoi[OdangChon] = 0;
                chay = true;
                ben_trai = true;
            }
            else
            {
                banchoi[OdangChon] = 0;
                chay = true;
                ben_trai = false;
            }
        }

        private void setAllbtn()
        {
            player1_trai.Enabled = false;
            player2_trai.Enabled = false;
            player1_phai.Enabled = false;
            player2_phai.Enabled = false;
        }

        private void setbtnPlayer1()
        {
            player1_trai.Enabled = true;
            player2_trai.Enabled = false;
            player1_phai.Enabled = true;
            player2_phai.Enabled = false;
        }

        private void setbtnPlayer2()
        {
            player1_trai.Enabled = false;
            player2_trai.Enabled = true;
            player1_phai.Enabled = false;
            player2_phai.Enabled = true;
        }
    }
}
