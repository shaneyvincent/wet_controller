# wet_controller
 Using filter methods, refactor out the repeated blocks of code.
SHANEYS HOMEWROK HELP

Check out this gist:
https://gist.github.com/tjr05d/03f6a9bb13ba33acc544

This controller’s actions have a lot of repeated code. Using filter methods, refactor the repeated blocks of code.


class UsersController < ApplicationController

 before_action :load_user, only: [:show, :edit, :create, :update, :destroy]

  def index
    @users = User.all
  end

  def show
  end

  def new
    @user = User.new
  end

  def edit
  end

  def create


    respond_to do |format|
      if @user.save
        format.html { redirect_to @user, notice: 'User was successfully created.' }
        format.json { render :show, status: :created, location: @user }
      else
        format.html { render :new }
        format.json { render json: @user.errors, status: :unprocessable_entity }
      end
    end

    Slack.notify_channel 
  end

  def update


    respond_to do |format|
      if @user.update(user_params)
        format.html { redirect_to @user, notice: 'User was successfully updated.' }
        format.json { render :show, status: :ok, location: @user }
      else
        format.html { render :edit }
        format.json { render json: @user.errors, status: :unprocessable_entity }
      end
    end

    Slack.notify_channel
  end

  def destroy


    @user.destroy
    respond_to do |format|
      format.html { redirect_to users_url, notice: 'User was successfully destroyed.' }
      format.json { head :no_content }
    end

    Slack.notify_channel
  end

  private

  def load_user
    @user = User.find(params[:id])
  end
    def user_params
      params.require(:user).permit(:username, :email)
    end
end
