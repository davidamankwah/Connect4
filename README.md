using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Xamarin.Forms;

namespace Connect4
{
    public partial class MainPage : ContentPage
    {
        const int MAX_ROWS = 6;
        const int MAX_COLS = 7;
        int turn;
        int colorturn = 1;
        Color piece = Color.Red;
        int row = 6;
        int col;
        BoxView bv;
        public MainPage()
        {
            InitializeComponent();
        }

        private async void TapGestureRecognizer_Tapped(object sender, EventArgs e)
        {

            turn = 1;

            bv = (BoxView)sender;
            // move the boxview.
            double yDistance = 5 * (GridC4.Height / 8);
            await bv.TranslateTo(0, yDistance, 600);
            bv.TranslationY = 0; // reset after translation to avoid unexpected moving
            bv.SetValue(Grid.RowProperty, 3);
            //bv.LayoutTo

            GridC4.RaiseChild(ImgC4Grid);   // raise the z axis value
            TapGestureRecognizer t = new TapGestureRecognizer();
            t.Tapped += TapGestureRecognizer_Tapped_1;

            bv.BackgroundColor = Color.Red;
            bv.HeightRequest = GridC4.Height / 8;
            bv.WidthRequest = GridC4.Width / 9;
            bv.CornerRadius = bv.WidthRequest / 2;
            bv.GestureRecognizers.Add(t);
            bv.SetValue(Grid.RowProperty, 0);
            bv.SetValue(Grid.ColumnProperty, 3);
            col = (int)bv.GetValue(Grid.ColumnProperty);

            GridC4.Children.Add(bv);
            GridC4.LowerChild(bv);


            DropPiece(row, col);
        }
        private void DropPiece(int r, int c)
        {
            if (turn == 1)
            {
                BoxView sq;
                sq = new BoxView();
                sq.BackgroundColor = Color.Red;

                sq.HeightRequest = GridC4.Height / 7;
                sq.WidthRequest = GridC4.Width / 6;
                sq.CornerRadius = sq.WidthRequest / 2;
                sq.SetValue(Grid.RowProperty, r);
                sq.SetValue(Grid.ColumnProperty, c);
                sq.StyleId = "BoardSquare";
                GridC4.Children.Add(sq);
                LblFeedback.Text = "Player1 turn";
                turn = 2;
            }

            else if (turn == 2)
            {
                BoxView q;
                q = new BoxView();
                q.BackgroundColor = Color.Yellow;

                q.HeightRequest = GridC4.Height / 7;
                q.WidthRequest = GridC4.Width / 6;
                q.CornerRadius = q.WidthRequest / 2;
                q.SetValue(Grid.RowProperty, r);
                q.SetValue(Grid.ColumnProperty, c);
                q.StyleId = "BoardSquare";
                GridC4.Children.Add(q);
                LblFeedback.Text = "Player2 turn";
                turn = 1;
            }

        }

        private async void TapGestureRecognizer_Tapped_1(object sender, EventArgs e)
        {

            BoxView b = (BoxView)sender;

            Rectangle originalRect = new Rectangle(b.X, b.Y, b.Width, b.Height);

            Rectangle smallRect = new Rectangle(b.X + b.Width / 2, b.Y,
                                                1, b.Height);

            await AnimateIn(b, smallRect);
            await AnimateOut(b, originalRect);
         

            --row;
            DropPiece(row, col);
           if (colorturn == 1)
            {
                b.BackgroundColor = Color.Red;
                colorturn = 2;
            }
            else if(colorturn == 2)
            {
                b.BackgroundColor = Color.Yellow;
                colorturn = 1;
            }

        }

        private Task AnimateIn(BoxView b, Rectangle r)
        {
            return b.LayoutTo(r, 350, Easing.SinIn);
        }

        private Task AnimateOut(BoxView b, Rectangle r)
        {
            return b.LayoutTo(r, 350, Easing.SinOut);
        }

       
    }
}
